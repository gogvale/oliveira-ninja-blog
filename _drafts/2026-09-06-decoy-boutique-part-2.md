---
title: "🍯 Honeypot v2: Crypto Boogaloo"
date: 2030-01-01 00:00:00 -0600
categories: [Security, Homelab]
tags: [security, honeypot, crypto, threat-intel, self-hosting, project-writeup]
description: "Part two of the honeypot story: a fake crypto-finance startup, credentials you can only guess by reading the business, and the hunt for the visitor that reads the page before it breaks in."
draft: true
---

<!-- DRAFT-ONLY NOTES (delete before publish):
STATUS: run two is LIVE — sixteen days in as of 2026-09-30. This draft follows the live-run rule:
what changed and what is being hunted, no counts, no results, never the domain or the host.
TITLE: Gabriel's pick, 2026-09-30 — "🍯 Honeypot v2: Crypto Boogaloo".
PROVISIONAL CUT (interim numbers, for the closing rewrite — do NOT paste until he calls the run closed):
  SSH 94,404 login attempts / 94,814 connections / 1 success and 1 command typed, both ours.
  Web external: 15,049 requests. Funnel external-only: /robots.txt 412 (144 addrs) → /sitemap.xml 227 (44)
  → /api/docs 98 (74) → /llms.txt 7 (7) → /agent-notes 0, /archive 0, /api/v1/agent-ack 0.
  Value surface: /api/v1/balance 10, /withdraw 9, /keys 5, /export-seed 3 (the honeytoken), 4 addresses.
  Fake-C2 sinkhole: 3,965 requests by class — plain web server 1,309 / other paths 764 / config+secrets+admin 641
  / TLS ClientHello on a non-TLS port 466 / silent 325 / AI-agent endpoints (/mcp, /sse, /v1/messages) 201
  / raw protocol pings 180 / embedded-device exploits 79.
  SSH crowding: 17 distinct addresses on day one → 284 on day fifteen; distinct addresses are the trend.
CHARTS (rendered 2026-09-30 with the interim cut, spec beside this file):
  honeypot-v2-sinkhole-classes (donut) · honeypot-v2-value-surface (donut) ·
  honeypot-v2-ssh-attempts-per-day (bar) · honeypot-v2-distinct-ips-per-day (bar) — PNG + Chart.js twin each.
  All four are labelled "first sixteen days (open run)". Regenerate from the closing cut and split into
  in-post mermaid where a pie fits. Part one's chart1..chart4 stay as they are — this set has its own names.
DESCRIPTION: carries no numbers on purpose until the closing cut — part one's numbers live in the description
  slot, not the H1, so add the closing figures here (126–183 chars).
IP POLICY (Gabriel, 2026-09-30): addresses and hostnames stay in the internal note, never in the post.
  Describe behaviour and the class of infrastructure. See Hermes/Fluvia-Actores-Interno.md.
BANNER + audio: pending, same treatment as part one (banner needs a real data visual, not text only).
SPLIT: part one shipped 2026-09-25 (the boutique). This is part two. The old combined
  _drafts/2026-09-06-decoy-boutique.md is superseded and can be deleted.
-->

## Where part one left off

Part one ended on the honest dead end: a fake textile store pulled in nothing but commodity noise, and every crowd that knocked was already documented in every feed you can subscribe to. Nobody read the shop. The one password I made guessable on purpose sat there for a week and stayed unguessed.

If someone interesting exists out there, a WordPress boutique will not find them.

So I kept the method and changed the prey.

## The new bait

A fake crypto-finance startup. It is the most targeted niche of the last few years, and its value surface is obvious to anyone who reads it: balances, withdrawals, an API, a wallet seed. That is the difference from the store — this business is worth understanding before you rob it.

The changes matter more than the story:

- **Credentials that need context.** The fake shell no longer accepts everything. The logins that work are derived from the site's own content — the kind of password you can only produce by reading the business, not by running a dictionary. Anyone who gets in is, by construction, someone who read the shop.
- **The seed is a honeytoken.** There is an export endpoint for a wallet recovery phrase. Whoever touches it came for the money, and touching it writes a line in the log.
- **C2 traffic gets answered instead of dropped.** When a visitor phones home, a sinkhole answers and keeps the conversation going, so the playbook runs to the end under observation instead of dying at the first step.
- **Scripted and agentic get separated.** Every session is scored on timing and navigation order — whether the thing walking through my fake startup is a bot with a script or a model reasoning its way through.

Same rule as last time: nothing on that box can call out. Every request gets logged instead of allowed, and the visitor never learns they were talking to a wall.

## What run two changed

Sixteen days in, the shape of the traffic is the opposite of run one. The boutique attracted volume and no attention. This one attracts less volume and more reading: visitors who fetch the API docs before they touch the API, and who ask for the endpoints those docs advertise instead of guessing at paths.

The planted ports tell their own story. Most of the traffic that finds them treats them as an ordinary web server — a root request, a favicon, a login page. A respectable slice arrives expecting secrets: config files, environment files, git directories, admin panels. Some arrive speaking TLS to a port that answers plain JSON, which means they believed they had found an HTTPS service and their playbook died mid-handshake. And a growing set asks for agent-shaped endpoints — the MCP and SSE and messages routes people use to wire models into tools. Nobody was looking for those two years ago.

The credential guessing changed too. Run one was one botnet after another, each hammering a dictionary. Run two is quieter and wider: crowds of a few hundred addresses, a handful of attempts each, spread across consumer ISPs instead of rented servers. Low and slow, from machines that look like somebody's living room. A campaign that used to be one loud IP is now a mailing list.

## Who is reading

One visitor class does the thing the whole lure was built for. It walks the value surface in order — balance, withdraw, webhooks, and the wallet seed export — from a residential address, with a browser user-agent, days apart, twice. It never exploits anything, never logs in, and never sends a withdrawal: the request fields come back empty. It read the page and asked for what the page advertised.

Whether that is a scraper, a researcher, or a bot wearing a browser is the thing I cannot prove yet, and I would rather say so than file it as an intrusion.

On the other side sit the verified crawlers, and they behaved. The mainstream AI crawlers fetched the robots file, then the sitemap, then stopped. Which means the honest headline of this run is the same as the last one, one level down: the visitor that took the bait was not a crawler. It was an automation pretending to be a browser.

## What I am still hunting

The run stays open until I have enough to answer the question part one could not: when something walks in and starts reading, is it a person, or a machine? The instrumentation is built for it — an agent-only runbook on the site, a behavioural score on every session, and a honeytoken that fires when someone reaches for the money.

Three things have to resolve before this becomes a post with an ending: what the address that read the seed is, whether anyone at all confirms the agent marker (so far, silence), and whether the quiet credential crowds behind those consumer addresses are one operator or many. I have my guess. Guessing is not the point.

Part two when the run closes.
