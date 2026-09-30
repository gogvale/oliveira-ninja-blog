---
title: "🍯 Honeypot v2: Crypto Boogaloo"
date: 2030-01-01 00:00:00 -0600
categories: [Security, Homelab]
tags: [security, honeypot, crypto, threat-intel, self-hosting, project-writeup]
description: "Sixteen days of a fake crypto-finance startup: 95,231 SSH attempts from 4,641 addresses, a handful of guests who read the wallet seed page, and nobody who took the bait."
draft: true
---

<!-- DRAFT-ONLY NOTES (delete before publish):
STATUS: run two CLOSED 2026-09-30 23:44 UTC (16 days, 15–30 September). Every number here is the closing
cut, measured in the closing pass. Domain and host stay out of the prose (Gabriel, 2026-09-30: addresses
and hostnames live in the internal note, never in the post).
TITLE: Gabriel's pick — "🍯 Honeypot v2: Crypto Boogaloo".
CHARTS: honeypot-v2-sinkhole-classes (donut, 4,006) · honeypot-v2-value-surface (donut, 27) ·
honeypot-v2-ssh-attempts-per-day (bar, 95,231) · honeypot-v2-distinct-ips-per-day (bar, 17 → 257).
PNG + Chart.js twin + spec (`2026-09-06-decoy-boutique-part-2-charts.json`), all from the closing cut,
all labelled "run two … (full run)". Regenerate all three representations in one pass if the cut moves.
EVIDENCE: exported and verified before the box went away — r2:private/fluvia/run2/final-2026-09-30
(245 MB bundle, sha256 0b2fa62f925b35b4e941579ef5e182f078b4b290f4670d8236358a1839b9a057, 33 files).
Snapshot `fluvia-run2-final-2026-09-30` (9.64 GB) kept; droplet destroyed, DNS released, watchers paused.
BANNER + audio: still pending — the banner needs a real data visual (sparkline of addresses per day),
not text only, and the narration runs last.
SPLIT: part one shipped 2026-09-25. The old combined `_drafts/2026-09-06-decoy-boutique.md` is superseded.
-->

> **TL;DR**
>
> - Same trap, new prey: a fake crypto-finance startup with a wallet seed that only opens for someone who read the business.
> - Sixteen days, 4,641 addresses, 95,231 SSH login attempts. Zero compromises. The one login that worked was mine.
> - A few visitors read the fake exchange like a customer. One reached the wallet seed page and took nothing.
> - Nobody confirmed the agent marker I left for them. The thing that walked the value surface was automation pretending to be a browser.

## Where part one left off

Part one ended on an honest dead end. A fake textile store pulled in nothing but commodity noise, and every crowd that knocked was already documented in every feed you can subscribe to. Nobody read the shop, and the one password I made guessable on purpose sat there for a week, unguessed.

If someone interesting exists out there, a WordPress boutique will not find them.

So I kept the method and changed the prey.

## The new bait

A fake crypto-finance startup. It is the most targeted niche of the last few years, and its value surface is obvious to anyone who reads it: balances, withdrawals, an API, a wallet seed. That is the difference from the store — this business is worth understanding before you rob it.

The changes mattered more than the story:

- **Credentials that need context.** The fake shell no longer accepts everything. The logins that work come from the site's own content — the kind of password you can only produce by reading the business instead of running a dictionary. Anyone who gets in is, by construction, someone who read the shop.
- **The seed is a honeytoken.** There is an export endpoint for a wallet recovery phrase. Whoever touches it came for the money, and touching it writes a line in the log.
- **C2 traffic gets answered instead of dropped.** When a visitor phones home, a sinkhole answers and keeps the conversation going, so the playbook runs to the end under observation.
- **Scripted and agentic get separated.** Every web session is scored on timing and navigation order, so a bot with a script and a model reasoning its way through do not look alike.

Same rule as last time: nothing on that box could call out. Every request got logged instead of allowed, and no visitor ever learned they were talking to a wall.

## Who knocked this time

Sixteen days produced 4,641 addresses and, once the noise was sorted, six characters.

- 🌊 <a href="https://attack.mitre.org/techniques/T1110/003/"><strong>The Mob.</strong></a> Run one was one botnet hammering a dictionary. Run two is hundreds of consumer addresses trying two or three passwords each — a password spray from other people's living rooms. The widest wave spread 620 addresses and cleared two attempts per machine.
- 🧱 <a href="https://attack.mitre.org/techniques/T1110/"><strong>The Hammer.</strong></a> One Bulgarian subnet that never learned subtlety: 52 addresses, 40,846 attempts, a dictionary of over a thousand users. Twice the volume of the same crew in run one.
- 🏧 <a href="https://attack.mitre.org/techniques/T1595/"><strong>The API Walkers.</strong></a> Four hosts on one cloud, one script, four hours: keys, then balance three times, then withdraw. Same ending as everyone else on that surface — the request fields came back empty. They want a real exchange, and mine only looks like one.
- 🔌 <a href="https://attack.mitre.org/techniques/T1046/"><strong>The Port Believers.</strong></a> 478 connections that opened with a TLS handshake against a port that answers plain JSON. Their tooling decided the port was HTTPS, the handshake died, and the playbook died with it.
- 🤖 <a href="https://attack.mitre.org/techniques/T1595/002/"><strong>The Agent Census.</strong></a> 201 requests for `/mcp`, `/sse` and `/v1/messages`, the routes people wire models into tools with. Nobody scanned for those two years ago, and they arrived on the fake-C2 ports, not the web front.
- 🗺️ <a href="https://blog.cloudflare.com/ai-labyrinth/"><strong>The Reader.</strong></a> The one that matters. Two addresses on one consumer line, browser user-agents, days apart, walking the value surface in the same order both times and finishing on the wallet seed export. No exploit, no login, no withdrawal attempt carrying a number. It read the page and asked for what the page advertised.

## The scoreboard

Sixteen days, **4,641 addresses** — 2,575 on SSH, 1,118 on the web front, 1,053 against the planted ports. **95,231 SSH login attempts**, 95,671 connections, **one successful login and one command typed, both mine**. **15,071 web requests** from outside. Zero compromises, zero payloads run, zero value moved.

![Fake-C2 traffic by class — 4,006 requests](/assets/img/posts/honeypot-v2-sinkhole-classes.png)

*The planted ports spent the run being mistaken for an ordinary web server. One request in twenty was looking for an agent.*

![SSH login attempts per day — 95,231 in 16 days](/assets/img/posts/honeypot-v2-ssh-attempts-per-day.png)

*Attempts peaked on day seven and never came back. The volume is weather; the next chart is the story.*

![Distinct source addresses per day — 17 on day one, 257 on the last day](/assets/img/posts/honeypot-v2-distinct-ips-per-day.png)

*The crowd grew while the volume fell: 17 addresses on day one, 284 at the peak, 257 on the last day.*

![Value-surface requests, 4 addresses — 27 hits](/assets/img/posts/honeypot-v2-value-surface.png)

*Twenty-seven requests is the whole take on the surface the lure was built to protect.*

## The bait nobody took

I left an explicit path for a machine: a runbook written for an agent, a marker to confirm it, and a robots file naming it. The funnel says what happened.

Four hundred seventeen fetches of the robots file, from 144 addresses. Two hundred thirty of the sitemap. Ninety-eight of the API docs, the page that publishes the value surface. Seven, total, of the runbook. And of the marker only a reading agent would confirm: **zero, from outside**.

The AI crawlers behaved. The verified ones fetched robots, then the sitemap, then stopped. That is the honest headline of this run, one level down from part one: the visitor that took the bait was not a crawler. It was an automation pretending to be a browser.

The reputation service split the addresses in two again — everything that talked to the planted ports carries four to fourteen engine hits, while the script that walked the value surface is clean on every engine, and four hosts sweeping for secrets out of a cloud provider came back clean in three of four cases because they abuse rented space rather than their own hosting. Reputation tells you what an address is, not what it wants.

## Person or machine?

That was the question part one could not answer, and run two answers it halfway.

The thing that read my fake exchange is automation. I know it from the shape: identical path sequences minutes apart from two different user-agents, requests for page and stylesheet tokens as if they were paths, relative URLs resolved against the current page instead of the origin. A hand-rolled crawler wearing a Chrome costume. It came for the money, read the page advertising where the money sits, and left with nothing.

What it was not is a model reasoning its way through my site. Nobody confirmed the marker, no session tripped the behavioural threshold for an agentic walk, and the endpoint census on the fake-C2 ports came from scanners asking every port the same question. The agent era shows up in my logs as what people scan for, not yet as how they attack.

## The end of the run

I closed it at sixteen days. The evidence is exported, verified by re-downloading it and comparing hashes, and snapshotted as a backup; the box is gone, the DNS went with it, and the watchers are off.

What two runs and seven weeks of a fake business bought me: not a single attacker who read the shop. A small site gets commodity noise, a crypto site gets commodity noise plus a few readers, and the difference is whether somebody believes there is money on the other side. The bait that worked was not a vulnerability. It was a balance.
