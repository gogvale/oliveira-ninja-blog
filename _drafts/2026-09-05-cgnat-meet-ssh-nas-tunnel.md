---
title: "Reach Your CGNAT NAS From a Cloud Server (and Fix the Slow Transfers)"
date: 2030-01-01 10:00:00 -0600
categories: [Self-Hosting, Linux]
tags: [self-hosting, linux, tutorial, ssh, cgnat, nas, entware]
description: "A step-by-step tutorial: outbound reverse SSH tunnel from a NAS behind CGNAT to a cloud server, transferring files through it, and the method used to hunt a 0.6 MB/s throughput wall."
draft: true
---

> **TL;DR**
> - Objective: a NAS behind CGNAT (no public IP) that accepts files from a cloud server — no inbound ports.
> - Method: outbound reverse SSH tunnel, keys both directions, a keep-alive loop that survives reboots.
> - Problem found along the way: transfers stall at ~0.6 MB/s no matter what.
> - Method for the problem: measure every layer until the wall is isolated — the result and final fix land at the end of this post.

## Objective

You end this tutorial with a NAS that a cloud server can reach and write to, through an encrypted tunnel the NAS itself maintains. No port forwarding, no public IP, no third-party daemon on the NAS. The NAS sits behind [CGNAT](https://www.cloudflare.com/learning/network-layer/what-is-cgnat/) (common with Starlink and many ISPs) — port forwarding is not just inconvenient, it is impossible.

**Prerequisites:** SSH access to the NAS as root. A cloud server with key-only SSH. This walkthrough uses a WD My Cloud EX2 Ultra (OS5) and a DigitalOcean server; the steps transfer to any NAS with SSH.

## Step 1 — Enable SSH and fix the two stock quirks

Enable SSH from the NAS dashboard. Then log in:

```bash
ssh root@<nas-ip>
```

Two quirks appear immediately on this firmware:

**Quirk 1: root's home is `/home/root`, not `/root`.** SSH looks for keys in `$HOME/.ssh`. Put them in the right place:

```bash
mkdir -p /home/root/.ssh && chmod 700 /home/root/.ssh
ssh-keygen -t ed25519 -N "" -f /home/root/.ssh/id_ed25519
cat /home/root/.ssh/id_ed25519.pub   # copy this to the server's authorized_keys
```

**Quirk 2: the stock sshd only allows the internal `sshd` user.** Root logins with keys get rejected. Allow root, then reload:

```bash
sed -i 's/^AllowUsers sshd/AllowUsers root sshd/' /etc/ssh/sshd_config
killall -HUP sshd
```

Authorize the server on the NAS too (append its public key to `/home/root/.ssh/authorized_keys`) so transfers do not need passwords.

## Step 2 — Open the reverse tunnel

From the NAS, dial out to the server. This makes `localhost:2222` on the server behave like port 22 on the NAS:

```bash
ssh -N -R 2222:localhost:22 hermes@vps.example.com
```

The tunnel dies with the terminal, so run it as a supervised loop that reconnects and survives logout:

```bash
cat > /home/root/keep_tunnel.sh <<'EOF'
#!/bin/sh
while true; do
  ssh -o ServerAliveInterval=30 -o ServerAliveCountMax=3 \
      -o ExitOnForwardFailure=yes -N -R 2222:localhost:22 hermes@vps.example.com
  sleep 10
done
EOF
chmod +x /home/root/keep_tunnel.sh
nohup /home/root/keep_tunnel.sh >/dev/null 2>&1 &
echo "@reboot /home/root/keep_tunnel.sh >/dev/null 2>&1 &" | crontab -
```

`ServerAliveInterval` keeps the connection honest; `ExitOnForwardFailure` refuses to pretend the forward works when it does not. `nohup` survives logout; the `@reboot` crontab entry restarts it after a NAS reboot.

## Step 3 — Transfer files through it

From the server, the NAS is one hop away:

```bash
sftp -P 2222 root@localhost
put "film.mp4" /mnt/HD/HD_a2/Peliculas/
```

## The problem: transfers stall at ~0.6 MB/s

A 200 MB test transfer takes 4 minutes 40 seconds. The uplink is capable of far more. The method for this kind of wall is **isolation: measure every layer until one measurement does not match the others.**

**Layer 1 — direction.** Measure upload and download through the tunnel. Both stall the same way. Not a direction problem.

**Layer 2 — TCP window.** High latency plus a small window is the classic cause on satellite links. Raise buffers and enable window scaling on the NAS (`sysctl`), restart the tunnel so a new connection negotiates, retest. Same wall. Not TCP configuration.

**Layer 3 — CPU and cipher.** The NAS negotiates only ancient ciphers (no chacha20, no AES-GCM). Benchmark the CPU before blaming it:

```bash
openssl speed -seconds 3 aes-128-cbc
# ~50 MB/s
```

Fifty megabytes per second of AES on a weak ARM core. Not the CPU. And a modern OpenSSH 10.2 client installed via Entware reproduces the same wall — not the SSH build either.

**Layer 4 — raw TCP.** The remaining suspect is the path itself: the NAS's internet uplink. Test plain HTTP, no SSH in the middle: serve a file from the server on a scratch port, pull it from the NAS with `wget`, and compare. This is the decisive measurement — the result fills in below.

## Fix

The wall turned out to be the ISP's per-flow QoS, not the NAS. The measurements that proved it: a laptop on the same Starlink link peaks at 8.3 Mbps upload; the NAS pulls 0.64 MB/s with one connection and 1.85 MB/s with four parallel ones. Per-flow ceiling of ~5 Mbps, aggregate ceiling of roughly 10-15 Mbps. No software change moves it.

The fix is therefore procedural, not technical:

**1. Parallelize transfers.** One connection wastes the link. Four to eight parallel pulls saturate it:

```bash
# from the server, push with N parallel streams
seq 1 8 | xargs -P8 -I{} sftp -P 2222 root@localhost <<< "put ..."   # or use aria2c/wget -c per file
```

**2. Accept the streaming ceiling.** One stream = one flow = ~5 Mbps. Lean 1080p encodes (~2-4 Mbps) stream fine from the NAS; the 15-45 Mbps remuxes in a typical library do not. Keep hot titles cached on the server's local disk; let the NAS hold the archive.

## Closing note

The tunnel pattern itself is solid and cheap: stock tools, no inbound exposure, zero cost. Whatever the wall turns out to be, the design does not change — only the expectations around transfer speed do.
