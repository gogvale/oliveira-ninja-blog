---
title: "Internet Honeypot 30-Day Experiment (pinned project)"
date: 2030-01-01 00:00:00 -0600
categories: [Homelab, Security]
tags: [pentesting, self-hosting, project-writeup]
description: "Pinned project — 30-day honeypot experiment measuring how AI-era automation changed opportunistic attacks, with an anti-detection decoy design."
draft: true
---

> **TL;DR** (fill at write-up time)
> - Pinned 2026-09-03, protocol drafted same day. Come back when time allows implementation.
> - Experiment: does 2026 AI-era attack automation differ from 2022-2025 baselines?
> - Requirements: anti-detection decoy (3-tier checks), byte-stable 30-day run, evidence shipped off-host.

## The experiment

**Research question:** how has opportunistic internet attack automation changed now that AI tooling is cheap and universal (2026), vs the pre-AI baselines (2022-2025 studies)?

**Pre-registered hypotheses:**
- **H1 — volume/credential patterns unchanged.** Bots scale, they do not think. Expect to replicate ambientnode (2025): ~1,900 attempts/day, ~20% default credentials, >80-90% automated, only ~1.6% of sessions >1 min.
- **H2 — AI-era signals appear in the tail.** Context-aware credentials (passwords matching the lure business), fewer classic defaults among *interactive* sessions, more staged/ordered recon instead of uname-spam, novel tool downloads.
- **H3 — detection-check pass rate rises.** A measurable share of sessions now run tiered anti-honeypot checks (cpu cores → env integrity → /proc entropy). Making the decoy pass those checks is a validity requirement: without it, sophisticated actors silently self-select out and the sample is biased toward naive bots.

**Limitations (state in write-up):** single host = anecdote; 30 days; confounds = region, provider reputation, DNS presence vs bare IP, uneven AI adoption across actors. One slice of opportunistic noise, not the whole internet.

## Anti-detection design (get best results)

Attacker checks, by tier (ohiiho, Jun 2026):
- **Tier 1 — commodity:** CPU-core counting, basic /proc reads, uname consistency
- **Tier 2 — environment integrity:** uptime sanity, cpuinfo realism, tool `--help` behavior, sandbox/container tells
- **Tier 3 — reality battery:** /proc entropy, PID-list reality, cross-referenced state

Static tells to eliminate:
- Cowrie default SSH host key is public → generate unique key
- SSH banner + algorithm list must match real OpenSSH (server-side HASSH fingerprint)
- "Always root, everything succeeds" behavior; no process tree; static command output (InnerWarden)
- Unrealistic/empty /proc + filesystem → coherent fake fs (InnerWarden list; MDPI survey)
- Web side: real Let's Encrypt certs on staging names, believable error pages, no perfect-uptime tells, consistent story across UI/API/exports/configs

Countermeasure stance: target **Tier 2 pass minimum, Tier 3 stretch**. Build a self-test harness that runs known detection routines (incl. Tier 2/3 patterns) against the decoy and fix failures BEFORE Day 5 launch. Byte-stable policy: after launch, no decoy changes for 30 days (a changed lure mid-run breaks comparability and re-triggers detection windows).

## Annotated sources

| Source | What we used it for |
|---|---|
| [ambientnode.uk — Cowrie data & findings (Zsolt Bizderi, 2025)](https://ambientnode.uk/running-a-cowrie-honeypot-data-and-findings) | Primary baseline: 89,109 events / 20,683 sessions / 2,123 IPs in 11 days; ~1,900/day; 12,199 logins; ~20% default creds; 1.6% sessions >1 min; 129 downloads; observed anti-honeypot probes; peak 15:00 UTC |
| [SANS ISC diary 33220 — "22 Seconds to Compromise" (May 2026)](https://isc.sans.edu/diary/33220) | Post-auth timing: automated login → persistence in 22s → Falco/auditd must catch live; sets observation cadence |
| [ohiiho research — Honeypot-Detection Arms Race (Jun 2026)](https://research.ohiiho.com/reports/2026-06-honeypot-detection-arms-race/) | 3-tier detection taxonomy → anti-detection requirements + H3 metric definition |
| [InnerWarden — Honeypot Attackers Can't Detect (Mar 2026)](https://www.innerwarden.com/blog/honeypot-attackers-cant-detect) | Countermeasure checklist: /proc coverage, dynamic outputs, fake filesystem |
| [MDPI Future Internet — Honeypot Fingerprinting review (Apr 2026)](https://www.mdpi.com/1999-5903/18/4/190) | Academic grounding for fingerprinting/detection countermeasures |
| [Resecurity honeytrap / ShinyHunters (Jan 2026)](https://www.darkreading.com/endpoint-security/scattered-lapsus-hunters-researcher-honeypot) | Lure credibility at pro tier; risk note: attackers bragged publicly when they smelled the trap |
| [Cowrie (GitHub/docs)](https://github.com/cowrie/cowrie) | Stack base; NOTE: Cowrie now has an LLM mode — decide inclusion as lure option or confound |
| [tamamo — LLM web honeypot (GitHub, Mar 2026)](https://github.com/secmon-lab/tamamo) | Reference for realistic admin-page decoys; our stance = deterministic (no LLM in lure path) |
| [Krawl-honeypot (GitHub + YT, Jan 2026)](https://github.com/0xSojalSec/Krawl-honeypot) | Closest OSS analog: fake site + real-time dashboard (IPs, paths, creds, UAs) |
| [arXiv 2104.03651 — "Escape the Fake"](https://arxiv.org/pdf/2104.03651) | Simulated container escapes → observe escape attempts without handing over the host |
| [Gcore — Trapping Hackers with Containerized Deception (2023)](https://gcore.com/learning/compromised-container-detection-with-honeypot-containers) | Containerized high-interaction honeypot design |
| [it-learn.io — Container Escape (Apr 2026)](https://blog.it-learn.io/posts/2026-04-19-container-escape-breaking-out-of-docker/) | Escape techniques + Falco detection rules |
| [Haroon — Cowrie analysis (2024)](https://medium.com/@abbasiharoon/cowrie-honeypot-analysis-5e1ab6ffce63) + [T-pot/Cowrie (2022)](https://medium.com/@ashlyncmatthews/t-pot-cowrie-honeypot-analysis-5e3793bb9128) | Older baselines (987 IPs/7d; 4,563 attacks/24h) for trend comparison; dashboard-workflow reference |
| ChatGPT session plan (PDF, "oliveira_honeypot_research_plan") | Initial architecture skeleton: droplet, subdomains, components, phases, checklist — cross-checked here |

## Step-by-step implementation plan

**Kickoff decisions (make these first, then freeze):**
- D1. Lure storyline: **e-commerce vs "Ledgerly" invoicing — pick ONE** (mixed artifacts break believability)
- D2. Subdomain set: staging/api/app-v2/dashboard/files (only those supporting the story)
- D3. Exposure route: droplet public IP direct (preferred; separate disposable droplet, real infra untouched)
- D4. Stack freeze: Cowrie (unique host key) + fake app (lightweight: nginx + minimal app or static+API stubs) + Falco + auditd + rotating tcpdump; remote evidence store = DO Spaces (or cheap object storage), NOT the real infra
- D5. Anti-detection target: Tier 2 pass mandatory, Tier 3 stretch (cost/benefit call)
- D6. Credential bait: include context-aware decoys (business-name passwords) as H2 probe
- D7. Cowrie LLM mode: include as a SEPARATE lure instance (confound control) or skip — decide
- D8. Analysis: heavy analysis local/post-experiment; dashboard during run stays lightweight
- D9. Region/provider: one basic DO droplet ($6-12/mo); note region as confound vs baselines
- D10. Baselines locked: ambientnode 2025 (primary) + Haroon 2024 + T-pot 2022
- D11. CT seeding: register subdomains + Let's Encrypt certs at launch (Day 5); check crt.sh appearance timing as a measured variable
- D12. IR/shutdown procedure documented + evidence export cadence (daily, automated)

**Phases:**
- **Day 0:** freeze decisions D1-D12. Write lure story bible (one page: business, users, artifacts, creds) so every artifact is consistent
- **Days 1-2 (build):** provision disposable droplet; firewall; install Cowrie (+custom key/banner), fake app, Falco, auditd, rotating capture; configure daily export to remote store
- **Days 3-4 (validate):** self-test anti-detection harness (run Tier 1-3 detection routines against own decoy; fix all failures); verify every log source; verify honeytokens; verify NO real creds/data on host; test restore + analysis workflow locally
- **Day 5 (launch):** DNS records + certificates; expose intended services; record baseline fingerprints (nmap self-scan, banner, HASSH); note crt.sh first-appearance timestamp
- **Days 6-28 (observe):** byte-stable. No decoy modifications. Daily automated evidence export; annotate major incidents; watch disk/egress; rotate captures
- **Day 29 (freeze):** stop changes; final full export (logs, artifacts, metadata, capture subset)
- **Day 30+ (analyze):** pull evidence local; build per-source timelines; cluster sessions; funnel login → enum → detect-check → download → persistence; compare against D10 baselines; flag anything absent from all baselines = the finding. Then write-up + dashboard post (pinned post outline in repo: "The Internet Screams at My Homelab")

## Metrics (log these, compare at the end)

- Baseline-replicable: attempts/day, unique IPs, % default creds, top-10 passwords/usernames, % sessions >1 min, top commands (rank-order vs ambientnode's uname-heavy list), peak-hour profile
- AI-era probes: context-aware credential attempts, detection-check attempts by tier, post-auth command clusters (kit similarity), download-hash diversity, natural-language/agentic session patterns, discovery-to-first-attack timing per subdomain (CT-log timestamp → first hit)
- Funnel rates: login success → enumeration → detect-check → file download → persistence attempt

## Status

Pinned + protocol drafted 2026-09-03. Next step when Gabriel has time: kickoff meeting on D1-D12, then Day 0.
