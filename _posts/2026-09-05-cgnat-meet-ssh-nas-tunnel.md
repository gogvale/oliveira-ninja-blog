---
title: "Reach Your CGNAT NAS From a Cloud Server"
date: 2026-09-05 19:25:00 -0600
categories: [Self-Hosting, Linux]
tags: [self-hosting, linux, tutorial, ssh, cgnat, nas, cloudflare, r2]
description: "A step-by-step tutorial: outbound reverse SSH tunnel from a NAS behind CGNAT to a cloud server, with keys both directions, R2 as the server's storage, and the NAS as the offsite mirror."
image: /assets/img/posts/2026-09-05-cgnat-meet-ssh-nas-tunnel.png
---

> **TL;DR**
> - A NAS behind CGNAT has no public IP, so the cloud server cannot reach it. Reverse the direction: the NAS dials out, the server gets a door back.
> - The server's family media library lives on Cloudflare R2, mounted as a disk with rclone — cheap storage with zero egress fees.
> - The NAS, at another house, is the offsite mirror. The server pushes copies to it over the tunnel, like it was a local disk.
> - Cost: a few dollars a month. Inbound ports opened: zero.

## Why

My family's media server runs on a cloud box. The library outgrew its local disk fast — movies, series, and the kids' cartoons add up, and a 77 GB droplet does not hold a collection. The answer was [Cloudflare R2](https://developers.cloudflare.com/r2/): object storage at $0.015 per GB, and egress costs nothing, which matters when four households stream from the same library.

The other half of the story is a NAS that sits behind [CGNAT](https://www.cloudflare.com/learning/network-layer/what-is-cgnat/), the thing ISPs use when they ran out of IPv4 addresses. No public IP, ever. Port forwarding is not difficult on that box — it is impossible. The NAS lives at my in-law's house: a different building, a different network, which makes it a perfect offsite archive for the library — if the server can reach it.

The classic answers all annoyed me. VPN clients on a firmware-locked NAS: fragile. DynDNS: does not fix CGNAT. Carrier-grade NAT means the port does not exist, so forwarding to it is theater.

SSH already ships everywhere. The trick is to stop trying to reach the NAS and let the NAS reach you.

## What you need

- A NAS with SSH enabled (any brand works; I use a WD My Cloud EX2 Ultra on OS5)
- A cloud server with key-only SSH (mine is a DigitalOcean droplet)
- A Cloudflare account with R2 enabled (free tier, pay per GB stored)
- Ten minutes

## Step 1 — SSH in, and fix the two things WD hides

Enable SSH from the NAS dashboard and log in. Then the firmware shows its personality.

**Quirk one: root's home is `/home/root`, not `/root`.** SSH looks for keys in `$HOME/.ssh`. Put them in the place the box checks:

```bash
mkdir -p /home/root/.ssh && chmod 700 /home/root/.ssh
ssh-keygen -t ed25519 -N "" -f /home/root/.ssh/id_ed25519
cat /home/root/.ssh/id_ed25519.pub   # → authorized_keys on the server
```

**Quirk two: the stock sshd only allows the internal `sshd` user.** My first root login worked; my first automated connection did not. One line fixes it:

```bash
sed -i 's/^AllowUsers sshd/AllowUsers root sshd/' /etc/ssh/sshd_config
killall -HUP sshd
```

Add the server's public key to `/home/root/.ssh/authorized_keys` on the NAS so transfers never ask for a password.

## Step 2 — Open the tunnel, backwards

From the NAS, dial out to the server. `localhost:2222` on the server now behaves like port 22 on the NAS:

```bash
ssh -N -R 2222:localhost:22 hermes@vps.example.com
```

That one line is the whole trick. The connection is outbound, so no router, no firewall rule, no public IP — the tunnel rides on the connection the NAS already made.

A tunnel dies with the terminal, so run it as a loop that reconnects and survives logout:

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

`ServerAliveInterval` keeps the connection honest. `ExitOnForwardFailure` refuses to sit there pretending the forward works when it does not. `nohup` survives you closing the laptop; the `@reboot` line brings it back after the NAS restarts.

## Step 3 — Give the server a disk that never fills

Create an R2 bucket, generate an API token scoped to that bucket (and locked to the server's IP, if you like), then mount it with rclone:

```bash
rclone config create r2 s3 provider Cloudflare \
  access_key_id "<key>" secret_access_key "<secret>" \
  endpoint "https://<account>.r2.cloudflarestorage.com"

rclone mount r2: /mnt/r2 --vfs-cache-mode=off --dir-cache-time=60s &
```

The server now sees an effectively unlimited disk at `/mnt/r2`. The media library, the Jellyfin metadata, everything points there. A few hundred gigabytes costs single-digit dollars a month, and every stream served costs nothing in egress — which is the whole point when the consumers are your relatives in three cities and one other country.

One detail that will bite you: rclone caches directory listings, so a file you just uploaded takes up to `--dir-cache-time` seconds to appear through the mount. Point a library scan at it too fast and the item will not be there yet. Wait a minute, then scan.

## Step 4 — Mirror to the NAS

The R2 library is the working copy. The NAS is the offsite insurance — a different building, a different ISP, a different failure domain. From the server, the NAS is one hop away:

```bash
sftp -P 2222 root@localhost
put "backup.tar" /mnt/HD/HD_a2/Archivo/
```

First try: file lands, checksum matches, done. The NAS still thinks it is a local appliance. The server treats it like a remote disk it owns — slow, but it does not need to be fast. It needs to be somewhere else.

## What you end up with

A library the whole family streams from anywhere, a NAS that receives offsite copies without a single inbound port open anywhere, and a monthly bill measured in single digits. The appliance never learns it is reachable from outside — because it is not. It is the one who calls home, and home answers on the other side of a door only the NAS can open.

The whole thing cost nothing but two config lines and a habit of reading `sshd_config` before blaming the router. If you want the industrial version of the keep-alive, [autossh](https://www.harding.motd.ca/autossh/) supervises the same loop. If your needs grow past one machine, [Tailscale](https://tailscale.com) is the mesh version of the same idea — I kept the tunnel because the NAS should not run more software than it already does.
