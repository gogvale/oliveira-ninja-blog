---
title: "I Ran a Honeypot Operation for 30 Days 🍯"
date: 2030-01-01 00:00:00 -0600
categories: [Security, Homelab]
tags: [security, honeypot, crypto, threat-intel, self-hosting, project-writeup]
description: "TBD — part 2 of the honeypot story: a fake crypto-finance startup, credentials that need context, and the hunt for an attacker who reads before it breaks in."
draft: true
---

<!-- DRAFT-ONLY NOTES (delete before publish):
PART 2 of 2. Do NOT publish while the crypto lure is live, and never name its domain or host. Everything here is what we built and what we are hunting — never results, until the run closes and Gabriel calls it.
TITLE: provisional, carried over from Gabriel's earlier pick. "30 days" is only accurate for the whole operation after ~Oct 6, 2026 (run 1 = Sep 7-13; run 2 opened Sep 15). Re-pick the title with Gabriel before this goes anywhere near a publish date.
Part 1 (the boutique, run 1) is 2026-09-06-decoy-boutique-part-1.md.
BANNER: pending, same social-card treatment as part 1.
-->

> **TL;DR** (fill at publish time — numbers come from the closing cut)

## Where part one left off

Part one ended with the honest dead end: a fake textile store pulled in nothing but commodity noise, and the crowds that knocked were already documented in every feed you can subscribe to. If someone interesting exists, a WordPress shop will not find them.

So I kept the method and changed the bait.

## The new bait

A fake crypto-finance startup. It is the most targeted niche of the last few years, and its value surface is obvious to anyone who reads it: balances, withdrawals, an API, a wallet seed. That is the difference from the store — this business is worth understanding before you attack it.

The changes matter more than the story:

- **Credentials that need context.** The fake shell no longer accepts everything. The only logins that work are derived from the site's own content — the kind of password you can only produce by reading the business, not by running a dictionary. Anyone who gets in is, by construction, someone who read the shop.
- **The seed is a honeytoken.** There is an export endpoint for a wallet recovery phrase. Whoever touches it is not exploring — they came for the money, and they get a log entry.
- **C2 traffic gets answered instead of dropped.** When an attacker phones home, a sinkhole answers and keeps the conversation going, so the playbook runs to the end under observation instead of being cut off at the first step.
- **Scripted and agentic get separated.** Every session is scored on timing and navigation order, which is the layer I care about: whether the thing walking through my fake startup is a bot with a script or a model reasoning its way through.

## What we are watching for

The run is still open, so this section stays thin until it closes. What matters now is the question part one could not answer: whether the thing on the other end of a login is a script or a model. The bait for that is explicit — there is a runbook on the site, written for an agent, that a human attacker has no reason to read.

What the closing cut will need: total requests and SSH sessions, how many credential attempts used the contextual story (the number that tells us whether anyone read the page), any honeytoken touches, and the agentic scores across sessions. Charts come from the same tooling as part one.

Part two when the fun ends.
