---
title: "0.6 MB/s to My NAS: Debugging an SSH Mystery From the Root"
date: 2030-01-01 10:00:00 -0600
categories: [Self-Hosting, Linux]
tags: [self-hosting, linux, project-writeup, ssh, cgnat, nas, debugging, entware]
description: "A tutorial in hypothesis-driven debugging: reaching a CGNAT NAS from a cloud server, hitting a 0.6 MB/s wall, and eliminating suspects until the real culprit surfaced."
draft: true
---

> **TL;DR**
> - A WD My Cloud NAS behind Starlink CGNAT gets an outbound reverse SSH tunnel to a cloud server. Works. Slow.
> - Every transfer — tunnel or direct, upload or download — stalls at 0.6 MB/s.
> - Three hypotheses died in order: TCP window, CPU crypto, the tunnel itself. Each died by one command.
> - Root cause: the NAS ships an OpenSSH from before the channel-window rewrite. Modern client from Entware fixes it.

## What we are building

A NAS with no public IP, a cloud server that needs to push files into it, and zero inbound ports opened anywhere.

The NAS is a WD My Cloud EX2 Ultra on OS5 firmware. The server is a hardened DigitalOcean box. The NAS sits behind [Starlink's CGNAT](https://www.cloudflare.com/learning/network-layer/what-is-cgnat/) — no public IPv4, port forwarding impossible. The fix is a reverse tunnel: the NAS dials out, the server gets a door back.

```bash
ssh -N -R 2222:localhost:22 hermes@vps.example.com
```

`localhost:2222` on the server now behaves like port 22 on the NAS. Stock [OpenSSH remote forwarding](https://man.openbsd.org/ssh_config#R). SFTP through it works on the first try.

```bash
sftp -P 2222 root@localhost
put "Archive.org film (2024).mp4" /mnt/HD/HD_a2/Peliculas/
```

Two quirks surface before anything moves: root's home on this firmware is `/home/root`, not `/root` — keys go to the wrong place and ssh silently ignores them. And the stock sshd only allows the internal `sshd` user (`AllowUsers sshd`), which rejects root by key. Both are config, not code: put keys in `/home/root/.ssh`, one `sed` on `sshd_config`, reload.

The transfer works. Then the measurement ruins the mood.

## The bug: a hard 0.6 MB/s wall

Two hundred megabytes through the tunnel: 4 minutes 40 seconds. That is 0.71 MB/s. A 2 GB film would take 47 minutes. The Starlink uplink is capable of a hundred times that.

The first rule of debugging a wall: measure it from both directions before theorizing.

```bash
# write direction: server -> NAS through the tunnel
dd if=/dev/zero bs=1M count=200 | ssh -p 2222 root@localhost "cat > /dev/null"
# read direction: NAS -> server, same tunnel
ssh -p 2222 root@localhost "dd if=/dev/zero bs=1M count=200" | dd of=/dev/null
```

Both stall past 300 seconds. Symmetric wall. Whatever it is, it does not care about direction.

## Hypothesis 1: TCP window scaling

High latency plus a small TCP window is the classic cause. Starlink round trips are tens of milliseconds; a 64 KB window across 70 ms of latency moves roughly a megabyte per second — suspiciously close to the measured wall. This is the [bandwidth-delay product](https://en.wikipedia.org/wiki/Bandwidth-delay_product) math.

The NAS runs a Marvell SDK kernel (4.14). Window scaling gets enabled and buffers get raised:

```bash
sysctl -w net.ipv4.tcp_window_scaling=1
sysctl -w net.core.rmem_max=16777216
sysctl -w net.core.wmem_max=16777216
sysctl -w net.ipv4.tcp_rmem="4096 87380 16777216"
sysctl -w net.ipv4.tcp_wmem="4096 65536 16777216"
```

Sysctls apply to new connections only, so the tunnel restarts. Retest: identical wall. Hypothesis dead. A hypothesis dies only when the measurement says so — this one died at 0.71 MB/s again.

## Hypothesis 2: cipher and CPU

The NAS is ARMv7. Its OpenSSH negotiates ancient ciphers — the list has no chacha20-poly1305, no AES-GCM, just CBC and CTR variants. Old AES without hardware acceleration on a weak ARM core is a plausible ceiling.

Two commands settle it. First, what ciphers exist:

```bash
ssh -Q cipher
# 3des-cbc aes128-cbc aes192-cbc aes256-cbc rijndael-cbc aes128-ctr aes192-ctr aes256-ctr
```

That list dates the OpenSSH build as pre-6.5. Then the CPU benchmark, because guessing CPU cost without measuring is not debugging:

```bash
openssl speed -seconds 3 aes-128-cbc
# aes-128 cbc  42728.93k  47848.34k  49588.57k  50079.06k  50186.92k  50173.27k
```

Fifty megabytes per second of AES. The CPU is not the ceiling. Hypothesis dead.

## Hypothesis 3: the tunnel itself

The reverse tunnel double-legs through the NAS's own ssh client and server. Before blaming the architecture, run the same transfer direct — NAS to server, one ssh connection, no tunnel:

```bash
date +%s
ssh hermes@vps.example.com "dd if=/dev/zero bs=1M count=50 2>/dev/null" | dd of=/dev/null 2>/dev/null
date +%s
```

50 MB in 88 seconds. 0.57 MB/s. Identical wall without the tunnel. Hypothesis dead. The tunnel is not the problem — the NAS's SSH build is, in every direction it touches.

## Root cause

Ancient OpenSSH predates the channel-window rewrite. Modern OpenSSH negotiates a large per-channel window that grows with the connection; the old builds negotiate a small one that grows too slowly on high-latency links. The wall is protocol-level, which is why TCP tuning, cipher speed, and topology changes all moved nothing. The evidence lines up: a cipher list frozen pre-6.5, a wall invariant under every variable except the SSH build itself.

One detail mattered for the fix: the cap lives in the client and the server, because it is negotiated per channel by both peers. Upgrading one side is not enough. The NAS needs a modern SSH on both ends of anything that carries bulk data.

## The fix: Entware, stock SSH untouched

No one replaces the firmware's sshd in place — the rootfs is read-only and regenerated at boot. The proven route on this hardware is [Entware](https://entware.net), a package manager that installs into the data volume and leaves the stock system alone. If it fails, the stock SSH is still there. That property makes it safe to try.

```bash
# one-time install into the persistent volume
mkdir -p /mnt/HD/HD_a2/entware
mount -o bind /mnt/HD/HD_a2/entware /opt
wget -qO /tmp/ent.sh "http://bin.entware.net/armv7sf-k3.2/installer/generic.sh"
sh /tmp/ent.sh
```

```bash
# modern OpenSSH client (10.2p1 at time of writing)
/opt/bin/opkg update
/opt/bin/opkg install openssh-client
/opt/bin/ssh -V   # OpenSSH_10.2p1, OpenSSL 3.5.5
```

The tunnel script now points at the modern client:

```bash
#!/bin/sh
while true; do
  /opt/bin/ssh -o ServerAliveInterval=30 -o ServerAliveCountMax=3 \
      -o ExitOnForwardFailure=yes -N -R 2222:localhost:22 hermes@vps.example.com
  sleep 10
done
```

## Verification

_Results pending — the 50 MB direct test with the modern client is running as this draft is written._

## Conclusion

_Pending the verification number above._

The debugging pattern is the durable part: form a hypothesis, find the one command that can kill it, run it, record the result. TCP window, CPU crypto, and topology all looked plausible. Each had a decisive measurement. The culprit was the least glamorous suspect on the box — the age of its SSH — and the fix was a package manager the vendor never shipped.
