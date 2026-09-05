---
title: "CGNAT, Meet SSH: Reaching My NAS From a Server I Control"
date: 2030-01-01 10:00:00 -0600
categories: [Self-Hosting, Linux]
tags: [self-hosting, linux, project-writeup, ssh, cgnat, nas, digitalocean]
description: "A local-only NAS behind CGNAT, a hardened cloud server, and one outbound SSH tunnel that connects them without opening a single port."
draft: true
---

> **TL;DR**
> - My WD My Cloud NAS sits behind Starlink CGNAT. No public IP, no port forwarding, ever.
> - I enabled SSH on the NAS and opened one **outbound reverse tunnel** to a DigitalOcean server I control.
> - The server now pushes my DVD rips to the NAS over SFTP through `localhost:2222` — nothing inbound, no router touched, no VPN client on the NAS.
> - The firmware fought back in two weird ways. Both were fun to solve.

The first login told me everything: `AllowUsers sshd`.

My brand-new SSH access to the NAS — enabled through a hidden URL in the firmware, because Western Digital does not put an SSH toggle in the OS5 dashboard — was useless for automation. The sshd config only permitted the user `sshd`. Not root. Not me. The box was telling me, politely, that it is an appliance, not a server.

I needed that appliance to accept files from the internet. Not the other way around.

## The problem: an appliance behind CGNAT

The NAS is a WD My Cloud EX2 Ultra. Armada 385, 1 GB of RAM, a 2015-era kernel, and a firmware that treats third-party packages like a security risk. It is a great disk with a mediocre computer bolted on. It also lives behind [Starlink's CGNAT](https://www.cloudflare.com/learning/network-layer/what-is-cgnat/), which means it has no public IPv4 address at all. Port forwarding is not difficult. It is impossible.

The machine I *do* control is the [DigitalOcean server that runs my assistant](https://blog.oliveira.ninja/posts/hermes-on-a-droplet/). Hardened, key-only login, sitting on the public internet. I wanted that server to push freshly ripped DVDs to the NAS. The classic answer — port forward, DynDNS, pray — was off the table.

## The move: reverse the direction

A reverse tunnel flips the connection. The NAS dials out to my server, and the server gets a door back:

```bash
ssh -N -R 2222:localhost:22 hermes@159.223.148.236
```

That one command makes `localhost:2222` *on the server* behave like port 22 *on the NAS*. Outbound-only, so CGNAT never notices. This is stock [OpenSSH remote forwarding](https://man.openbsd.org/ssh_config#R) — no new software on the NAS, no Tailscale install on a locked firmware, no Cloudflare daemon to babysit. The NAS stays an appliance. It just happens to dial home once.

SFTP over the tunnel works with zero extra setup:

```bash
sftp -P 2222 root@localhost
put "Movie (2000).mp4" /mnt/HD/HD_a2/Peliculas/
```

The server pushes, the NAS stores. Inbound ports: zero.

## The firmware fought back twice

**First: root's home is not `/root`.** On WD's OS5, root's home is `/home/root`. My keys went to `/root/.ssh` and ssh pretended they did not exist, because ssh looks in `$HOME/.ssh`, and `$HOME` is `/home/root`. The verbose flag caught it in one line: `identity file /home/root/.ssh/id_ed25519 type -1`.

**Second: `AllowUsers sshd`.** Even with the key in the right place, the server rejected it. WD's sshd only lets the internal `sshd` user connect — their remote-support backdoor, presumably. One `sed` fixed it, plus a `killall -HUP sshd` to reload:

```bash
sed -i 's/^AllowUsers sshd/AllowUsers root sshd/' /etc/ssh/sshd_config
```

Two hours of my life, two lines of config. This is why I write things down.

## Keeping the tunnel alive

A foreground tunnel dies with the terminal. The NAS needed something that survives logouts and reboots. WD has no systemd and no autossh, so the classic loop it is:

```bash
#!/bin/sh
while true; do
  ssh -o ServerAliveInterval=30 -o ServerAliveCountMax=3 \
      -o ExitOnForwardFailure=yes -N -R 2222:localhost:22 hermes@159.223.148.236
  sleep 10
done
```

`ServerAliveInterval` keeps the connection honest; `ExitOnForwardFailure` refuses to sit there pretending if the forward fails. Run it with `nohup`, register it with `crontab -e` under `@reboot`, done. If the tunnel drops, the loop reconnects in ten seconds. If you want the industrial version, [autossh](https://www.harding.motd.ca/autossh/) does the same with supervision.

## The one thing still open

Raw throughput over the tunnel is capped at roughly 0.7 MB/s. That is not the link — Starlink gives me plenty of upload. It is the math of [bandwidth-delay product](https://en.wikipedia.org/wiki/Bandwidth-delay_product): a 64 KB TCP window across ~70 ms of satellite latency moves about one megabyte per second, no matter how fat the pipe is. The NAS kernel predates window scaling defaults I take for granted. The fix is sysctl tuning on the NAS side — [larger receive and send buffers](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/net.html), which I am testing now. For one-off DVD rips at 2 GB each, even 0.7 MB/s is tolerable. For streaming the library later, it is not. That is the next fight.

## Why not the obvious alternatives

Tailscale is the honest answer for most people: mesh VPN, no config, works through CGNAT, and it is what I would run if the NAS served multiple remote users. My case is narrower — one server pushing files to one appliance — and Tailscale still needs *something* installed on the NAS. The tunnel needs nothing but the SSH that WD already ships. Cloudflare Tunnel is the same story one level up: a daemon plus a domain, worth it when the NAS itself must serve the internet. It does not need to. My server does that job.

## What changed

My DVD rips — the ones from discs we actually own — now flow from the server to the NAS over an encrypted channel that leaves no inbound ports open anywhere. The NAS still thinks it is a local appliance. The server treats it like a remote disk. And the whole thing cost nothing but two config lines and a firewall rule I did not have to write.
