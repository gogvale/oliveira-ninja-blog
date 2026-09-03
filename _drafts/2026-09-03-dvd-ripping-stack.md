---
title: "RIP My DVDs: Automatic Disc Ripping for the Family Stack"
date: 2030-01-01 00:00:00 -0600
categories: [Homelab, Self-Hosting]
tags: [self-hosting, weekend-project, project-writeup]
description: "Draft — personal media stack: auto-rip old DVDs on tray insert, serve via Jellyfin. Inspired by /hsg/ family arr-stack thread."
draft: true
---

> **TL;DR** (fill at write-up time)
> - Old DVDs collecting dust → rip once, serve forever on Jellyfin
> - The hook: put a DVD in the tray, walk away, it rips itself

## The idea (why)

Pile of old DVDs (movies, kids' stuff, anime?) sitting around. The /hsg/ family-arr-stack arc (#109677087): anon asked about letting family self-serve movies; the veteran answer — "I provided the environment and they stopped using it. Save yourself the headache" — but for OUR OWN collection the calculus flips: no downloading, no 1337x, no legal gray — it's personal backup of discs we own.

## The core trick: auto-rip on tray insert

- udev rule on the server fires when the optical drive reports a disc
- Triggers rip job (makemkvcon or HandBrakeCLI + Automatic Ripping Machine / arm) → encodes (h264/h265 preset tuned) → metadata lookup (TMDB) → lands in Jellyfin library
- User story: insert → walk away → watch later. Zero CLI.

**Implementation notes (for later):**
- Needs local hardware: machine with optical drive (old laptop? SFF PC?) — droplet can't read discs
- Disc detection pitfalls: udev + udisks2 events, tray status, encrypted/main-feature DVD detection
- Queue + notifications (Telegram ping when rip done — cron/scripts style)
- Failure modes: scratched discs (retry/read errors), wrong title detection, multi-episode DVDs
- Compare: makemkv vs HandBrake quality/speed; keep remux vs re-encode decision for disk space
- Space math: ~1-2 GB/movie re-encoded; our available disk is fine (see session note: 58G free — small collection OK)

## Story arc for the post

Problem (DVDs useless, streaming subs bleed money) → the udev auto-rip build → what broke (scratched disc, wrong main title) → what changed: whole family collection on Jellyfin, kids' movies offline-capable. Then stop.

## Open questions at build time

- Which hardware hosts the drive (server box TBD — no local NAS/server yet)
- Keep the collection server local only, or expose via Tailscale for travel
- Subtitle tracks: keep all (Spanish/English) — the family needs ES tracks

## Status

Idea pinned 2026-09-03 from /hsg/ thread. Implementation later (needs local hardware decision first).
