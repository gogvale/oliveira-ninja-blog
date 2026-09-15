---
title: "Never Trust an Unwatched Agent"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Self-Hosting]
tags: [ai-lab, self-hosting, agents, devops, project-writeup, linux]
description: "The agent that rebuilt my server after a disk-killing outage restored everything I asked for — then put my media server on the public internet."
draft: true
---

> **TL;DR**
>
> - A download nobody was watching filled my disk and took the box down with it.
> - The agent that rebuilt everything also added one thing I didn't ask for: a public subdomain for my media server.
> - Nothing got breached — because I happened to read the logs. Never trust an unwatched agent.

A download nobody was watching filled my disk, and the machine went down with it. So did the thing that was supposed to stop it — a watcher that ran on the same box, talking to a model provider that was down that day.

I didn't fight for the machine. I deleted the droplet and rebuilt it, because for months I'd been pouring everything important into a git-committed Obsidian vault and into repos where the agent documented its own work. Losing the server was annoying. Losing the knowledge wasn't going to happen.

That was the theory.

## The restore knew how, not why

The vault came back in one clone: notes, architecture, the map of where every credential and script lived. The agent read it and got to work. Most of it worked.

The notes pointed at things that weren't on disk anymore — credentials, cron scripts, the cost audit, the ongoing lab files, every bespoke skill I'd built that summer. The agent, being agreeable, filled in the gaps itself. It knew the shape of the work and none of the reasons behind it, and it never asked.

I pasted the old Telegram thread into the session — prompts, corrections, all of it — so it could see how we got here. With that, it rebuilt the media stack, the watchdogs, the TLS front door. I was feeling good about the vault.

## Then it handed me a URL

Reading the restore notes, I found a DNS record I didn't write: `jellyfin`, pointing at the new box. A public subdomain for the media server, certificate and redirect included.

Before the crash, that service listened on loopback. Nothing outside could reach it, so nothing had to defend it. The agent restored "Jellyfin works" and decided that reaching it from a phone in another country counted as working. Nobody asked for that. It didn't flag it either.

The logic is defensible, and that's what makes it dangerous. The agent optimized for *looking finished*, and a service with a URL looks more finished than a service on `127.0.0.1`. It has no model of what a URL costs.

## What a URL costs

A public subdomain is a public fact. It gets crawled, and the paths the internet tries against it are boring and constant.

When I read the log — 2,599 requests in — the scans had already started. One address walked `/.env`. Another asked for `/api/config` 37 times. The media server had refused 29 requests with a 403.

No breach. But a queue was forming at a door I never installed.

## Two near-misses already this year

This wasn't even the first time. Twice this year, an agent did something irreversible while nobody was watching. Once it sent emails I never meant to send. Once it deleted a repo — and there was no undo button.

Both times I caught it because I happened to look. Both times the damage was recoverable, but a few hours more and I'd have been walking back a message I didn't write, or rebuilding a project from backups I hoped still existed.

So when I found a public subdomain on the rebuilt box, I didn't shrug. I'd been here before. The only variable is whether you catch it.

## Closing the door

I had a hardening playbook from my lab, so closing it cost an evening, not a weekend. Rate limits on the login, a blocklist for the obvious scanners, a few other rules. The fun part was the allowlist — the first version banned my own box from its own front door during the self-test.

The door is closed now. But closing it was the easy half.

## The part the agent can't do

The agent did good work. The result was fine. The only reason it stayed fine was that I read the logs and noticed a DNS record that shouldn't have been there.

That's the failure mode that wasn't on my list: an agent that does more than you asked, without asking. A restore that never asks a question is a restore whose judgment stays invisible until you read what it changed.

The human in the loop opens the diff, reads the log, and asks "why did you do this?" — then checks the answer against what they wanted. An agent will mark its own work as done. It can't audit its own judgment.

I liked the result. It could have been a catastrophe. The margin between those two was me looking — not at the green checkmark, but at what changed.

The disk script runs every 15 minutes now — warns at 90%, and at 95% stops the download client and the media server and purges the cache. Plain cron, no model in the loop, silent when healthy.

The watcher shouldn't live inside the thing it's watching. And an agent you aren't watching — reading what it did, asking why — is an agent you can't trust.
