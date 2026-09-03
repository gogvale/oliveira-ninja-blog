---
title: "The Internet Screams at My Homelab: 30 Days of Honeypot Data"
date: 2030-01-01 00:00:00 -0600
categories: [Homelab, Security]
tags: [pentesting, self-hosting, project-writeup]
description: "Pinned idea — decoy services collecting real attack data from the internet, turned into a dashboard and a writeup."
draft: true
---

> **TL;DR** (fill at draft time)
> - Idea pinned 2026-09-03: run internet-facing decoy services, collect 30 days of real attack data, publish the dashboard + findings.

## The idea (pinned)

Replicate common attacks against commonly found misconfigurations — on purpose, with decoys. The homelab becomes a honeynet:

- Decoy services on a DMZ: SSH (Cowrie), a fake NAS admin panel, TPOT honeypot, maybe a fake OPNsense/WordPress login
- Internet-facing is the point (it is the one homelab project where exposure is the feature)
- Collect 30 days of real data: attacker IPs, countries, tooling fingerprints, passwords tried, CVEs probed
- Ship a dashboard (exec-dashboard muscle) + writeup: "What tried to break into my homelab this month"

## Why this one

- Uniquely me: AppSec SME + dashboard craft + real telemetry — not a done-to-death setup post
- Fresh data every day; numbers over adjectives
- Replicating attacks on real-world misconfigs = fun + endless material

## Open questions to solve at build time

- Isolation: decoys must have zero path to the real LAN (separate VLAN/VM, no credentials shared)
- Legal/hosting: own IP + authorized = fine; keep evidence scope clean (no engagement with attackers, no beacons)
- Provider: home public IP vs tunnel (Cloudflare/Tailscale funnel) to keep the house dumb
- Collection stack: Cowrie logs → parser → dashboard (deterministic scripts; LLM only if it earns a spot)

## Outline for the eventual post

1. Hook: the first credential pair an attacker typed into my decoy
2. Setup: DMZ decoys, why each one, how isolation is guaranteed
3. 30 days of data: top attacks, top IPs, the misconfigs they hunt
4. The dashboard
5. What surprised me / what changed in my real setup

## Status

Pinned idea — not started. Next step when picked up: decide honeypot stack + exposure route.
