---
title: "I Am Not a Threat Hunter. I Identified a Botnet Anyway."
date: 2030-01-01 00:00:00 -0600
categories: [Security, AI]
tags: [threat-intel, ai-collaboration, honeypot, security, workflow]
description: "How a non-specialist, with an AI as pair analyst, went from zero threat-intel experience to naming an attacker (mdrfckr/Outlaw) by its persistence key in a weekend — and the workflow that made it possible."
draft: true
---

<!-- PROVENANCE (strip at publish): idea seed from Gabriel, 2026-09-07. Real workflow: the Cotton Sky honeypot run (see the decoy post). Never name live lure domains/IPs. Publish AFTER the 30-day honeypot results post (this is the "how I did it" companion to "what I found"). 4chan never a source. -->

> **IDEA SEED — outline, not a draft yet.**

## The hook (authentic, on-brand)
"I am not a threat-intel specialist. I never worked in a SOC. I understand the basics — ports, credentials, the difference between a scanner and a breach — and the rest I learned on the fly, one question at a time, with an AI as my pair analyst. Within a weekend I went from watching bots hammer an SSH port to naming one of them by the RSA key it has reused since 2018."

## The arc (what actually happened)
1. Deploy a honeypot (fake store) → noise floods in within minutes
2. Capture everything (SSH sessions, web requests, credentials) → the raw logs
3. Ask: who are these people? → cluster the traffic by behavior (6 clusters, 6 motives)
4. OSINT the clusters: VirusTotal + GreyNoise + web → one IP is a known C2, four families co-hosted on one abusive provider
5. Attribute one: a planted RSA key's fingerprint → web search → "mdrfckr / Outlaw" → Kaspersky/Elastic writeups → it's a documented mining botnet, same key since 2018
6. Map to MITRE ATT&CK → motives, TTPs, targets
7. Derive the insight (all traditional, zero AI-era attackers) → feed a post

## Why it works as a post (the angle)
- It inverts the "you need to be an expert" gate: the workflow is learnable, and the AI does the heavy lifting on the parts a non-specialist lacks (OSINT recall, ATT&CK mapping, triage)
- It's the honest counter to the "10x AI" hype: not "AI made me a hacker" but "AI was a good pair analyst — I still had to ask the right questions"
- It demystifies threat intel for other developers who are curious but intimidated
- It's the "how" companion to the honeypot "what" post — process vs findings, zero overlap

## Open decisions (with Gabriel)
- ~~Timing~~ → **CONFIRMED (Gabriel, 2026-09-07): publish AFTER the 30-day honeypot post**, so it can reference the run without spoilers.
- ~~"ONE single honeypot post" rule~~ → **RELAXED (Gabriel, 2026-09-07):** parallel learning posts are welcome. The goal is learning; ideas/experiences that run parallel to the honeypot get registered as their own posts.
- Technical depth: keep it "method + a few concrete wins" (the mdrfckr fingerprint, the C2, the MITRE table) — not a tutorial

## Reference notes
- The full workflow + findings live in the honeypot-ops skill (`references/attacker-census-day1.md`, `run-findings.md`, `attacker-profiles.md`)
- Candidate sources (re-ground later, never 4chan): Kaspersky Securelist Outlaw, Elastic Security Labs, SOCRadar WP Botnet Master, MITRE ATT&CK technique pages
- Pull-quote candidate: "You do not need a SOC to name a botnet. You need a question, a log, and someone willing to look up the answer with you."
