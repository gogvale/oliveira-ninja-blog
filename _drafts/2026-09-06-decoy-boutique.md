---
title: "I Ran a Honeypot Operation for 30 Days 🍯"
date: 2030-01-01 00:00:00 -0600
categories: [Security, Homelab]
tags: [security, honeypot, wordpress, self-hosting, project-writeup]
description: "I built two fake businesses, pointed them at the internet, and logged everything that came through the door: 51,561 web requests, 29,481 SSH connections, zero attackers who read the menu."
draft: true
---

<!-- DRAFT-ONLY NOTES (delete before publish):
TITLE SET BY GABRIEL — "30 days" is accurate only once the whole operation passes ~Oct 6, 2026 (run 1 = Sep 7–13, run 2 opened Sep 15). If part 1 publishes before that, either wait for run 2 to close (~Oct 3) or soften to "for a month". His call.
SPLIT POINT: this draft is written as a two-parter. Part 1 ends at "Then it got boring". Part 2 is the crypto run — it is STILL RUNNING, so everything about it here is what we changed and what we are hunting, never results. Do not name the live lure's domain/host while it runs.
Run-1 numbers are the closing cut (Sep 12, 15:53 UTC, the last report before the box was snapshotted and destroyed). chart1 is a five-day mid-run timeline — extending it needs hourly logs that died with the box. Project post: publish + banner only (no audio, no LinkedIn). -->

> **TL;DR**
> - A honeypot is an application that looks breakable, is not, and writes down every step of anyone who walks into it.
> - I'm not a threat hunter. Everything I knew about attacks came from newsletters, so I built my own warzone to see what actually turns up at the door — and how much of it is AI-powered now.
> - Run one: a fake WordPress boutique, seven days. 51,561 web requests, 29,481 SSH connections, 805 addresses, zero compromises.
> - The result is boring and it is the whole point: nobody attacked my store. They attacked WordPress. Baseline hygiene is the entire fight.

## What a honeypot actually is

A honeypot is an application that looks vulnerable and is not. It looks like a shop with a weak admin password, or a server where SSH takes any credential you throw at it. It is actually locked down, with every request, command, and connection written to a log — while an attacker believes they are moving through a real system.

That's the whole trick. You don't out-defend the attackers. You give them somewhere to be interesting.

## Why I wanted a warzone

Everything I know about cyber attacks came from newsletters. A new botnet, a new ransomware crew, a new CVE in something everybody runs. I read all of it and had no idea what it looks like from the inside. So I asked the three questions the newsletters never answer:

- What kind of attacks would actually hit *me*?
- How many of them are AI-powered now?
- How would I defend against this, and how common is it really?

I'm not a threat hunter. I write application code, I think about access control and secrets hygiene for a living, and that's it. So I leaned on AI heavily to get here: learning the tooling, building the lure, wiring the monitoring, and translating what my day job taught me about appsec security into something that gets hammered by the actual internet. Then I applied what I already knew and let the experiment run.

The first victim was a shop that never existed.

## Run one: the boutique

I built a WordPress store selling home textiles. WooCommerce, ten products with photos, prices that look right — a duvet at $96, a pillow cover at $28. A real one-click cloud install, so the processes and the database look like the thousands of small stores installed exactly the same way.

Then the bait:

- A customer database full of invented people — 20 customers, 46 orders going back years, reviews, addresses. All fake. If someone dumps that table, they have stolen nothing and I have learned which tables they reach for.
- A guessable admin password, derived from the store's own story. The kind a bored owner sets in 2019 and never changes.
- SSH with a filter set to accept *anything*. Every username, every password, all of it correct as far as the server was concerned — and every command typed after that recorded.
- And no outbound road. Nothing on that box could call home, download a payload, or join a botnet. Every attempt got logged instead of allowed.

One thing I did not do: promote it. No ads, no backlinks, no social, no directory submission. A new domain and an open port.

## Who knocked

I expected scanners. I did not expect characters. Within a week, the traffic sorted itself into people I could name, and I started rooting for some of them:

- 🕵️ **The Screener.** Runs one command — `uname`, the Linux version of "who are you?" — against every host it can reach, then leaves. 11,544 of my SSH connections were this machine taking inventory. It is the census taker of the internet.
- 💓 **The Beacon.** Logs in, types `echo xsec`, hangs up. A heartbeat to confirm the shell works and phone it home. 7,090 visits. It never looked at the shop.
- ⛏️ **The Miner.** The most interesting one, because it is a real, documented criminal operation. It plants an SSH key so it can come back, then leaves a CPU check behind. Its key has not changed since 2018. Eight years, same trick, no arrests. By the end it had planted that key 110 times on my fake shop.
- 🚇 **The Tunnel Hunters.** They wanted no data and no money — they wanted my box as a relay, a hop that hides where their traffic really comes from. Seven attempts at the same outside address. That's how an innocent server becomes an accessory.
- 🌊 **The XML-RPC Flood.** Wave after wave against WordPress's automation endpoint, each burst from a freshly rented server, gone before the invoice arrives. 35,758 requests, 69% of everything that hit me.
- 🧱 **The Login Stuffer.** Drips a username at the login page for hours, rotating browsers to look human, hiding behind a proxy. One of them posted 81 username-and-password pairs from a real breach — a Thai gambling site's leak, pointed at a textile store in Oregon. It never read a single product page.
- 🪤 **The Backdoor Artist.** The only one with craft: it checks whether the shell it landed in is real before installing a fake one of its own. On a real host that would matter. Here it verified a shell that does not exist.
- 🛒 **The Shopper.** The one I can't explain. It browsed the catalog like a customer — products, cart, checkout, even the lost-password page. Every other visitor hammered the plumbing. This one behaved like it wanted to buy a duvet. Best guess: an automated agent pacing itself like a human. I can't prove it, and it's the closest anything came to noticing what the shop was for.

## The parts that stuck with me

**Speed.** The first SSH attempt landed 0.1 seconds after I opened the port — before the domain existed, before the certificate existed. The first crawl looking for my domain came 1.5 hours after the certificate was issued. Nobody had my address. The internet does not need your address.

**Nothing brought them.** No ads, no backlinks, no visitors. A WordPress site does not need an audience to be attacked. It needs to exist.

**It doesn't matter how small you are.** I was a fake shop with no traffic and my own invented customer list, and I still got 1,720 password attempts in seven days. If you run a small business site, you are not too small to be in this dataset. You are the dataset.

**The bait never got eaten.** The password I deliberately made guessable — derived from the store's own About page, the thing a human who read my site would try — was tried ten times in seven days, and never correctly. Across 913 credential pairs, nobody tried the story. The closest anyone got was the brand and the right year, lowercase, no exclamation mark. Three keystrokes short. Nobody read the shop. Not once.

As a sanity check on the "AI will crack your password" argument, I ran it backwards: a frontier model, told the username and handed the store's own content, got ten guesses and zero correct. Its third guess was one capital letter and one symbol away. Even the model needed the story — and no attacker on the wire used it.

**The mix flipped at the end.** For the first four days, port 22 was passive: scan, run `uname`, leave. In the closing 48 hours that reversed — 8,985 of 10,028 sessions stayed long enough to type commands. A new brute-force botnet showed up with sixteen addresses out of one subnet. A multi-architecture malware family started uploading itself in five builds at once. Same target, harder shoves.

![SSH sessions, final 48 hours — 8,985 of 10,028 reached the fake shell and ran commands](/assets/img/posts/chart2-ssh-clusters.png)

*The mix flipped in the last two days: most of the week was inventory-taking, then nine in ten sessions stayed to run commands.*

**How much of it was AI?** None that I can prove. Every session in run one was traditional automation — scripted logins, scripted commands, technique that predates my career. The one agentic-looking visitor is a question mark, not a finding. If there are AI-powered attacks in the wild, they are not spending themselves on a textile store.

**The scoreboard.** Seven days, 51,561 web requests from 805 addresses, 29,481 SSH connections, 0 compromises, 0 attackers who read the store, 0 post-login activity. The 46 fake orders sat untouched.

![Web and SSH traffic, hour by hour (UTC) — first five days](/assets/img/posts/chart1-volume-timeline.png)

*Web traffic left, SSH right. The bursts are campaigns; the flat stretches are the internet idling.*

![Requests by target — 51,561 total](/assets/img/posts/chart3-web-classes.png)

*Almost seven in ten requests went to an endpoint built for automation, not for shoppers.*

**And the takeaway I keep coming back to.** Every single intrusion attempt died against basic hygiene: strong credentials, no exposed secrets, patched plugins, nothing listening outbound that shouldn't be. The numbers say the same thing from the other side: WordPress disclosed 11,334 vulnerabilities in 2025, and 91% of them were in plugins — six in the core ([Patchstack](https://patchstack.com/whitepaper/state-of-wordpress-security-in-2026/)). Wordfence's independent count for 2024 lands at 96% plugin-side. The compromise path is the layer nobody patches. The sophisticated stuff was real, and it was aimed at the author's target list, not at me. What hit me was the mop-up crew, and the mop-up crew is enough to end a small business.

One side finding worth keeping: when I ran the attacker addresses past VirusTotal, the two populations split cleanly. The SSH botnets are dirty — 7 of 8 addresses flagged, several detections each. The web-campaign infrastructure is the opposite: freshly rented, clean, unremarkable. VirusTotal will flag the next SSH wave and shrug at the next XML-RPC one. Your logs are the only registry that will ever have those addresses.

## Then it got boring

By day five the attacks stopped being a story and turned into weather. The same words, different addresses. The wave even died on its own — my web traffic fell from 12,896 requests in a day to 2,222, an 82% collapse, with nothing changed on my end.

That boredom was the answer to question one, and it was also a dead end. I had learned what knocks on a small shop, and it was all commodity noise that's already documented in every threat feed you can subscribe to. Nobody in that crowd was worth studying.

So I changed the strategy. If the interesting attackers exist, they aren't going to bother with a textile store, and they aren't going to guess a password from a wordlist. I needed a target that attracts people who read the site before they attack it.

## Part two: the crypto run

Same idea, different bait. A fake crypto-finance startup — the most targeted niche of the last few years, and a business model where the value surface is obvious: balances, withdrawals, an API, a wallet seed.

The changes matter more than the story:

- **Credentials now need context.** The fake shell no longer accepts everything. The only logins that work are derived from the site's own content — the kind of password you can only produce by reading the business, not by running a dictionary. Anyone who gets in is, by construction, someone who read the shop.
- **The seed is a honeytoken.** There's an export endpoint for a wallet recovery phrase. Whoever touches it is not exploring — they came for the money, and they get nothing but a log entry.
- **C2 traffic gets answered instead of dropped.** When an attacker phones home, a sinkhole answers and keeps the conversation going, so we watch the whole playbook instead of cutting it off.
- **We can tell scripted from agentic.** Every session gets scored on timing and navigation order, which is the layer I actually care about: whether the thing walking through my fake startup is a bot with a script or a model reasoning its way through.

It's four days in. No breaches, no guesses against the contextual credentials, and the traffic is the expected crowd — plus two things worth watching: a visitor that walked the API, the balance endpoints, the seed endpoint, and the admin path in careful order, and a crawler that came back and read the page content this time. Whether either of them is a machine that reads before it strikes is exactly the question the first run couldn't answer.

Part two when the fun ends.

*Draft note: THE single honeypot post — one start-to-finish story, told as two parts if it runs long. NON-technical (keep MySQL/PHP-FPM/Caddy/Falco/auditd internals OUT unless Gabriel changes his mind). The pre-registered protocol, hypotheses and metrics live in the honeypot-ops skill, not in this post.*
