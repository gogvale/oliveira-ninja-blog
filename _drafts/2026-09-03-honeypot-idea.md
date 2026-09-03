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

## Similar work & calibration (researched 2026-09-03)

Baseline plan doc came from a ChatGPT session (e-commerce lure variant on a basic droplet). These real-world writeups calibrate expectations:

**Volume expectations (ambientnode.uk, 11 days, Aug-Sep 2025):**
- 89,109 events / 20,683 sessions / 2,123 unique IPs (~1,900 attempts/day, ~78/hr)
- 12,199 login attempts; ~20% default credentials (root:root #1, admin:admin #2)
- >80-90% automated (Go scanners + PuTTY banners, identical HASSH fingerprints)
- **Only 1.6% of sessions lasted >1 minute** — interactive sessions are rare; mass automation disconnects in seconds
- 129 file-download events (malware delivery attempts real); some bots run honeypot-detection checks
- Peak hour ~15:00 UTC

**Automation speed (SANS ISC diary 33220, May 2026):** automated SSH actor went login → persistence in **22 seconds**. Post-compromise observation windows are short — Falco/auditd must catch it live.

**Lure credibility:** Resecurity (Jan 2026) trapped ShinyHunters/Scattered Lapsus$ with a coherent fake IdP + Mattermost decoy — validates the believable-story lure; attackers bragged publicly when they smelled the trap (incident-response note).
- tamamo (GitHub, Mar 2026): LLM-generated realistic admin-page decoys — reference for realistic signatures; our stance stays deterministic.
- Krawl-honeypot (GitHub + YT, Jan 2026): fake-site + real-time dashboard — closest OSS analog to the dashboard plan.
- Container angle: arXiv 2104.03651 "Escape the Fake" (simulated container escapes — supports observe-don't-hand-over approach); Gcore/itnext containerized deception; it-learn.io Apr 2026 (escape techniques + Falco rules).

**Gap this project fills:** no public study combining subdomain-lured fake SaaS + post-compromise enumeration + container-escape observation in one 30-day run with a dashboard.

**Storyline consistency check:** ChatGPT plan = "Ledgerly" invoicing SaaS; user framing = e-commerce. Pick ONE coherent lure story before Day 0 — mixed artifacts (invoice app + shop paths) break believability.

## Status

Pinned idea — not started. Next step when picked up: decide honeypot stack + exposure route.
