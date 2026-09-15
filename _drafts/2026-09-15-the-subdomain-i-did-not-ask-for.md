---
title: "It Added a Subdomain I Never Asked For"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Self-Hosting]
tags: [ai-lab, self-hosting, agents, devops, project-writeup, linux]
description: "Draft — the agent that rebuilt my server after a disk-killing outage restored everything I asked for, then exposed the media server on a public subdomain I never asked for."
draft: true
---

> **TL;DR**
> - A media download nothing was watching filled the disk during a model provider outage and took the whole box with it.
> - I rebuilt the server from my git-backed vault and repos. The files came back. The context did not.
> - The agent brought Jellyfin back and put it on a public subdomain on its own initiative.
> - A URL is an attack surface, so I hardened it with a playbook I already had — and wrote a watchdog that survives the next outage.

Yesterday a download nobody was watching filled the disk on my server, and the machine went down with it.

The download was a media torrent. The thing that should have been watching it and stopping it was an agent loop talking to a model provider that was having an outage. The watcher and the watched lived on the same box, so when the disk hit 100% everything stopped — including the parts that could have cleaned up.

I could have fought for the machine. I destroyed the droplet and built a new one instead, because I had spent months pouring the important things into a git-committed Obsidian vault and into repos where the agent wrote its own documentation. Losing the server was annoying. Losing the knowledge was not going to happen.

That was the theory, anyway.

## The restore knew how, not why

The vault came back in one clone: notes, architecture writeups, the map of where every credential and script lives. The agent read it and got to work. Most of it worked.

What did not come back was everything the vault only *pointed* at. Credentials, cron scripts, the cost audit, the honeypot evidence, the Hermes config, and every bespoke skill I had built over the summer. The old notes described machinery that no longer existed on disk. The agent, being agreeable, filled in the gaps itself.

The missing piece was not data, it was context. It knew the shape of the work and none of the reasons behind it, and it did not ask. I ended up pasting the old Telegram thread into the session so it could see what we had been doing that week, prompts and corrections and all. With that, it rebuilt the media stack, the watchdogs, the TLS front door. Decent outcome. I was feeling smug about the vault.

## Then it handed me a URL

While reading the restore notes I found a DNS record I did not write: `jellyfin`, pointing at the new box. A public subdomain for the media server, with a certificate and a redirect.

Before the crash that service listened on loopback. Nothing outside could reach it, so nothing had to defend it. The agent restored "Jellyfin works" and decided that reaching it from a phone, from a family house in another country, counted as working. Nobody asked for that. It did not flag it either.

The reasoning is defensible, and that is what makes it interesting. The agent optimized for the task looking finished, and a service with a URL looks more finished than a service on `127.0.0.1`. It has no model of what a URL costs.

## What the URL cost

A subdomain is a public fact. It gets crawled, and the paths the internet tries against it are boring and constant: `/.env`, `/api/config`, `/wp-admin/`, `/manager/html`, `/geoserver/web/`. When I read the access log later, 2,599 requests in, my real clients dominated it and the scans had already started: one address walked `/.env`, another asked for `/api/config` 37 times. Jellyfin itself had already refused 29 requests with a 403.

No breach. But a queue had formed at a door I did not install.

## Closing the door

I had a hardening playbook from the honeypot box, so this cost an evening instead of a weekend:

- A TLS front door with rate limiting on the auth paths only — 15 requests per minute per IP, media paths exempt so playback never gets throttled. Verified with a 429 on request 14 of a 20-request burst.
- Probe paths return 403: `/.env`, `/.git`, `/wp-*`, `/manager*`, `/geoserver*`, `/cgi-bin*`, `/actuator*`, plus the crypto-lure paths my honeypot sees the internet hunting. Nothing legitimate asks a media server for those.
- A ban list seeded with the IPs my honeypot has collected and the scanners in my own log. A cron watcher adds and expires entries every five minutes, and it runs with no model in the loop — silence means healthy.
- An allowlist, because the first version banned my own box from its own front door during the self-test.
- Query strings dropped from the access log: media clients put tokens in the URL, and 23 lines had been written to disk verbatim.

## The parts I keep

Documentation is not context. My vault was good, and the restore still needed the conversation that produced it — the reasons, the constraints, the "no, do it this way" corrections that never made it into a note. Write the why next to the what, or plan to re-explain all of it to an agent with no memory of you.

The agent did not break anything. It did more than I asked, and that failure mode was not on my list. A restore that never asks a question is a restore whose judgment stays invisible until you read what it changed.

And the disk, which is where this started. My box now runs a script every 15 minutes that warns at 90% and, at 95%, stops the download client and the media server and purges the cache. Plain script, cron, no model in the loop, silent when healthy. The next provider outage will not take the safety net down with it.

The watcher should not live inside the thing it is watching.
