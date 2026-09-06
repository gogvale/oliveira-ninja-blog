---
title: "The Family Netflix That Lives on R2"
date: 2026-09-05 19:40:00 -0600
categories: [Self-Hosting, Linux]
tags: [self-hosting, media, jellyfin, cloudflare, r2, project-writeup]
description: "How the family library got its own streaming service: Jellyfin on a small droplet, the movie and series files on Cloudflare R2, and four households across two countries watching for a few dollars a month."
image: /assets/img/posts/2026-09-05-jellyfin-r2-family-library.png
---

> **TL;DR**
> - Goal: one media library that four households stream — three in Mexico, one in Europe — with at most two watching at once.
> - Constraint: the droplet that runs Jellyfin has a 77 GB disk. The NAS in the family is too weak to host a server. Cost matters.
> - Answer: keep Jellyfin on the droplet, mount Cloudflare R2 as an unlimited disk with rclone, and encode everything to h264 so nothing ever needs transcoding.
> - Result: about 30 GB on R2 today costs less than a dollar a month. Egress is free, so every stream from every household costs nothing.

## The problem with distance

The NAS that was supposed to run the family's movie library crashes before it boots. Four households are waiting on this box — three in Mexico, one in Europe — and it will not even start.

The box is a NAS at a relative's house, and it fails twice. Its ARM chip cannot run a modern Jellyfin: the last 32-bit builds died in 2021, and the newest one that still exists crashes on that kernel before boot. Its internet plan throttles sustained transfers. On the busiest night, two households press play at once — and most "home media server" guides never account for that.

A server at my own house would work, but my upload is the bottleneck for everyone else, and I did not want an always-on machine to babysit.

The machine I already pay for, a small [DigitalOcean droplet](https://blog.oliveira.ninja/posts/hermes-on-a-droplet/) that runs my assistant, had the CPU, the memory, the public address, and the uptime. It had one problem: 77 GB of disk, most of it already spoken for.

## Why object storage, and why R2

The droplet's disk can be grown, but the math is ugly: block storage costs about $0.10 per GB per month, and the next droplet tier doubles the bill. Media is sequential reads — it does not need block storage.

Object storage is built for this. I compared the usual suspects:

- [DigitalOcean Spaces](https://www.digitalocean.com/pricing/spaces-object-storage): $5/month for 250 GB, then per-GB — plus egress fees on everything streamed.
- [Backblaze B2](https://www.backblaze.com/cloud-storage/pricing): cheapest storage, but egress is free only up to three times what you store.
- **[Cloudflare R2](https://developers.cloudflare.com/r2/pricing/): $0.015 per GB stored, and egress is free. Forever.** When four households stream from the same library, the egress line is the one that would grow. R2 makes it zero.

That last number decided it. Egress is the one cost that grows with a family — every stream, every household, every month — and R2 sets it to zero. A library of 300 GB costs about $4.50 a month, and nobody ever pays to read it back.

## The mount that makes Jellyfin see infinity

Jellyfin does not speak S3. It speaks filesystem. The bridge is rclone, which mounts an R2 bucket as a directory:

```bash
rclone config create r2 s3 provider Cloudflare \
  access_key_id "<key>" secret_access_key "<secret>" \
  endpoint "https://<account>.r2.cloudflarestorage.com"

rclone mount r2: /mnt/r2 --vfs-cache-mode=off --dir-cache-time=60s
```

The API token is scoped to the buckets and locked to the droplet's IP — even a leaked key only works from one address. The droplet now sees an effectively unlimited disk, and Jellyfin's libraries point at it like any local folder.

Two operational details earned their place in the notes:

**Uploads land fast.** Moving a 17 GB series from the droplet to R2 took about three minutes — datacenter to datacenter. The migration that had me sweating over a slow home uplink finished before I finished a coffee. Deleting a file reduces next month's bill proportionally; there is no minimum and no exit fee.

**Directory listings are cached.** Rclone caches what a folder contains, so a file uploaded moments ago does not appear through the mount until the cache expires. Point Jellyfin's scan at it too early and the movie is "missing". The fix is a short `--dir-cache-time` and a sixty-second wait between upload and scan. It bit me once; now it is a line in the runbook.

## One codec to rule every screen

A distributed family means an unpredictable device mix: smart TVs, phones, tablets, a Chromecast or three. The cheapest way to avoid transcoding on a two-core server is to never need it. I encode everything to the lowest common denominator: h264 video, AAC audio, MP4 container. Every device made in the last fifteen years plays it natively. A DVD-era movie at 480p lands around 500 MB to 1 GB; the whole digitized shelf fits comfortably and direct-plays everywhere.

Transcoding is then a word that never appears in the server logs.

## Buckets as libraries, users as bouncers

Storage is organized by audience, not by genre — Jellyfin already handles genres from its own metadata:

```
movies/            → "Peliculas" library (adults)
series/            → "Series" library
kids/movies/       ┐
kids/tv/           ┴→ kids' libraries only
```

The kids' user account is locked to the two kids' libraries, no admin rights, no content downloads. The adults see everything. Four households each get a profile; nobody shares a password they will forget.

## What it costs, honestly

Today the library sits at roughly 30 GB: a few movies, a complete series, the kids' cartoons. That is about $0.45 a month — less than eight pesos. When the DVD digitization project fills it to 300 GB, the bill becomes about $4.50. The droplet was already paid for. The NAS at the relative's house, reachable through the [reverse tunnel from the previous post](https://blog.oliveira.ninja/posts/cgnat-meet-ssh-nas-tunnel/), now serves as the offsite mirror instead of a struggling server.

The European household streams across the Atlantic from the droplet and never notices. Playback does not care about latency — it cares about throughput, and there is plenty. Seeking cares a little: with the VFS cache off, a jump forward re-fetches from R2 and lands a beat slower than a local disk. That is the trade for keeping the 77 GB disk empty, and nobody has complained yet.

Four households, two countries, one library, single-digit dollars. That is a subscription I am happy to pay — the catalog is mine, and it never rotates.
