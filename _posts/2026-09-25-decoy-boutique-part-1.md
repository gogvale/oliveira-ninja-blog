---
title: "I Spent a Week Watching Bots Try the Front Door of a Store That Doesn't Exist"
date: 2026-09-25 12:00:00 -0600
categories: [Security, Homelab]
tags: [security, honeypot, wordpress, self-hosting, project-writeup]
description: "I built a fake WordPress store, pointed it at the internet with no ads and no audience, and logged everything that came through the door: 51,561 web requests, 29,481 SSH connections, 805 addresses, and zero attackers who read the shop."
image: /assets/img/posts/2026-09-25-decoy-boutique-part-1.png
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/decoy-boutique-part-1.mp3">Your browser does not support the audio element.</audio>

> **TL;DR**
>
> - A honeypot is an app that looks breakable and isn't — it just writes down every move of whoever walks in.
> - I'm not a threat hunter. I built my own warzone to see what shows up at the door.
> - Run one: a fake WordPress boutique, seven days. 51,561 web requests, 29,481 SSH logins, 805 addresses, zero compromises.
> - Nobody robbed the store. They attacked WordPress. Baseline hygiene is the whole fight.

## What a honeypot is

A honeypot is a shop with a weak admin password, or a server where SSH takes any password you throw at it. It looks breakable. It isn't. Every request, every command, every login gets written to a log — while whoever's on the other end believes they're moving through a real system.

That's the whole trick. You don't out-defend attackers. You give them somewhere to be interesting.

## Why I wanted a warzone

Everything I know about cyberattacks came from newsletters. A new botnet, a new ransomware crew, a new hole in something everybody runs. I read all of it and had no idea what it looks like from the inside. So I asked the three questions the newsletters never answer:

- What kind of attacks would actually hit *me*?
- How many of them are AI now?
- How would I defend against this, and how common is it really?

I'm not a threat hunter. I write application code, I think about access control and secret hygiene for a living, and that's it. So I leaned on AI to get here: learning the tooling, building the lure, wiring the monitoring. Then I applied what I already knew and let the experiment run.

The first victim was a shop that never existed.

## Run one: the boutique

I built a WordPress store selling home textiles. Ten products with photos, prices that look right — a duvet at $96, a pillow cover at $28. A real one-click cloud install, so the processes and the database look like the thousands of small stores installed exactly the same way.

Then the bait:

- A customer database full of invented people — 20 customers, 46 orders going back years, reviews, addresses. All fake. If someone dumps that table, they've stolen nothing and I've learned which tables they reach for.
- A guessable admin password, derived from the store's own story. The kind a bored owner sets in 2019 and never changes.
- SSH set to accept *anything*. Every username, every password, all of it correct as far as the server was concerned — and every command typed after that, recorded.
- And no way out. Nothing on that box could call home, download a payload, or join a botnet. Every attempt got logged instead of allowed.

One thing I did not do: promote it. No ads, no backlinks, no social. A new domain and an open port.

## Who knocked

I expected scanners. I did not expect characters. Within a week the traffic sorted itself into people I could name, and I started rooting for some of them:

- 🕵️ <a href="https://attack.mitre.org/techniques/T1082/"><strong>The Screener.</strong></a> Runs one command — `uname`, the Linux version of "who are you?" — against every host it can reach, then leaves. 11,544 of my SSH connections were this machine taking inventory. The census taker of the internet.
- 💓 <a href="https://attack.mitre.org/techniques/T1071/"><strong>The Beacon.</strong></a> Logs in, types `echo xsec`, hangs up. A heartbeat to confirm the shell works and phone it home. 7,090 visits. It never looked at the shop.
- ⛏️ <a href="https://attack.mitre.org/techniques/T1496/"><strong>The Miner.</strong></a> The most interesting one, because it's a real, documented criminal operation. It plants an SSH key so it can come back, then leaves a CPU check behind. Its key hasn't changed since 2018. Eight years, same trick, no arrests. By the end it had planted that key 110 times on my fake shop.
- 🚇 <a href="https://attack.mitre.org/techniques/T1090/"><strong>The Tunnel Hunters.</strong></a> They wanted no data and no money — they wanted my box as a relay, a hop that hides where their traffic really comes from. Seven attempts at the same outside address. That's how an innocent server becomes an accessory.
- 🌊 <a href="https://attack.mitre.org/techniques/T1110/"><strong>The XML-RPC Flood.</strong></a> Wave after wave against WordPress's automation endpoint, each burst from a freshly rented server, gone before the invoice arrives. 35,758 requests, 69% of everything that hit me.
- 🧱 <a href="https://attack.mitre.org/techniques/T1110/004/"><strong>The Login Stuffer.</strong></a> Drips a username at the login page for hours, rotating browsers to look human, hiding behind a proxy. One of them posted 81 username-and-password pairs from a real breach — a Thai gambling site's leak, pointed at a textile store in Oregon. It never read a single product page.
- 🪤 <a href="https://attack.mitre.org/techniques/T1105/"><strong>The Backdoor Artist.</strong></a> The only one with craft: it checks whether the shell it landed in is real before installing a fake one of its own. On a real host that would matter. Here it verified a shell that doesn't exist.
- 🛒 <a href="https://blog.cloudflare.com/ai-labyrinth/"><strong>The Shopper.</strong></a> The one I can't explain. It browsed the catalog like a customer — products, cart, checkout, even the lost-password page. Every other visitor hammered the plumbing. This one behaved like it wanted to buy a duvet. Best guess: an automated agent pacing itself like a human. I can't prove it, and it's the closest anything came to noticing what the shop was for.

## The parts that stuck with me

**Speed.** The first SSH attempt landed 0.1 seconds after I opened the port — before the domain existed, before the certificate existed. The first crawl looking for my domain came 1.5 hours after the certificate was issued. Nobody had my address. The internet doesn't need your address.

**Nothing brought them.** No ads, no backlinks, no visitors. A WordPress site doesn't need an audience to be attacked. It needs to exist.

**It doesn't matter how small you are.** I was a fake shop with no traffic and my own invented customer list, and I still got 1,720 password attempts in seven days. If you run a small business site, you're not too small to be in this dataset. You are the dataset.

**The bait never got eaten.** The password I made guessable on purpose — derived from the store's own About page, the thing a human who read my site would try — was tried ten times in seven days, and never correctly. Across 913 credential pairs, nobody tried the story. The closest anyone got was the brand and the right year, lowercase, no exclamation mark. Three keystrokes short. Nobody read the shop. Not once.

![Login attempts at close — 913 pairs, 0 hits](/assets/img/posts/chart4-usernames.png)

*Most of the guessing stayed inside the brand-name-year family. Nothing in it came from the page it was sitting on.*

I even ran the "AI will crack your password" argument backwards: I handed the store's own content to a top AI model and asked it to guess the password. Ten guesses, zero hits. Its third guess was one capital letter and one symbol away. Even the model needed the story — and no attacker on the wire used it.

**The mix flipped at the end.** For the first four days, port 22 was quiet: scan, run `uname`, leave. In the closing 48 hours that reversed — 8,985 of 10,028 sessions stayed long enough to type commands. A new brute-force botnet showed up with sixteen addresses out of one subnet. A multi-architecture malware family started uploading itself in five builds at once. Same target, harder shoves.

![SSH sessions, final 48 hours — 8,985 of 10,028 reached the fake shell and ran commands](/assets/img/posts/chart2-ssh-clusters.png)

*The mix flipped in the last two days: most of the week was inventory-taking, then nine in ten sessions stayed to run commands.*

**How much of it was AI?** None that I can prove. Every session in run one was traditional automation — scripted logins, scripted commands, technique that predates my career. The one agentic-looking visitor is a question mark, not a finding. If there are AI attacks in the wild, they're not spending themselves on a textile store.

**The scoreboard.** Seven days, 51,561 web requests from 805 addresses, 29,481 SSH connections, 0 compromises, 0 attackers who read the store, 0 post-login activity. The 46 fake orders sat untouched.

![Web and SSH traffic, hour by hour (UTC) — first five days](/assets/img/posts/chart1-volume-timeline.png)

*Web traffic left, SSH right. The bursts are campaigns; the flat stretches are the internet idling.*

![Requests by target — 51,561 total](/assets/img/posts/chart3-web-classes.png)

*Almost seven in ten requests went to an endpoint built for automation, not for shoppers.*

**The takeaway I keep coming back to.** Every intrusion attempt died against basic hygiene: strong credentials, no exposed secrets, patched plugins, nothing listening outbound that shouldn't be. Almost every WordPress hole lives in a plugin nobody patches, not in the core itself. The sophisticated stuff was real — and it was aimed at the author's target list, not at me. What hit me was the mop-up crew, and the mop-up crew is enough to end a small business.

One more thing worth keeping: I checked the attacker addresses against a threat database, and they split clean in two. The SSH botnets were filthy — most flagged. The web-campaign servers were freshly rented and clean. Your logs are the only place those addresses will ever be written down.

## Then I changed the bait

By day five the attacks stopped being a story and turned into weather. Same words, different addresses. The wave even died on its own — my web traffic fell from 12,896 requests in a day to 2,222, an 82% collapse, with nothing changed on my end.

That boredom was the answer to question one. What knocks on a small shop is commodity noise, already documented in every feed you can subscribe to. The interesting attackers don't bother with a textile store, and they don't guess passwords from a wordlist.

So I changed the bait. Same trap, different prey: a crypto-finance startup — the kind of business you have to read before you rob it. Balances, an API, a wallet seed. And this time the door only opens for someone who read the story, not someone running a dictionary.

That hunt is still running. It's the second half of this story — and it's where the real question finally gets asked: when something walks in and starts reading, is it a person, or a machine?
