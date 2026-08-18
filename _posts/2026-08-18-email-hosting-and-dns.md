---
title: "Setting up oliveira.ninja, part 2: email, tradeoffs, and DNS"
date: 2026-08-18 01:20:00 -0600
categories: [Personal Site, Email]
tags: [domain, dns, email, zoho, proton, weekend-project]
description: "Why I moved from Proton to Zoho for email, and what I learned while wiring the DNS records."
---

After buying `oliveira.ninja`, I wanted email on the domain. That felt like the next natural milestone: owning the domain is nice, but sending and receiving mail from it makes it feel much more real.

My first stop was Proton.

## The Proton detour

I bought the Duo plan because I was planning to share it with my wife. The privacy story was good, the bundle looked complete, and it seemed like a clean way to get mail plus a bunch of related tools in one place.

Then I spent about a day exploring what was included.

That is when I realized I would not actually use most of it.

Not because the tools were bad. They just overlapped with things I already had and liked:

- Bitwarden already covered password management for me.
- My NAS already covered a lot of what I wanted from cloud drive.
- My Outlook plan already included 1 TB of storage.
- Google Authenticator was already handling my two-factor setup.
- Codex was already my AI tool of choice, so Lumo was not really filling a gap.
- LibreOffice covered my office-suite needs, and I do not really use office tools in the cloud.

The plan was good, but it was too much plan for my actual use.

Thankfully, Proton had a 30-day money-back guarantee.

## Finding the smaller fit

For this project, I really only needed domain email.

Zoho's free tier was enough: six users is more than I need, and the UI is honestly not that ugly. That may sound like a low bar, but for a free email admin panel, "not that ugly" is a legitimate feature.

So I switched plans: use Zoho for the domain mailbox and keep the rest of my existing tools.

This was a useful reminder that bundles are tempting, but replacement cost is not just money. It is also migration time, habit changes, and mental overhead.

## The DNS part

Then came the fun part: DNS records.

Zoho's docs were actually very helpful. They gave me the records I needed, and Porkbun made it easy enough to add them.

The setup included the usual email pieces:

- MX records so mail knows where to go.
- SPF so senders can be validated.
- DKIM so outgoing mail can be signed.
- DMARC so receiving systems know what policy to apply.
- A few verification records to prove I owned the domain.

This is the part where DNS becomes both simple and annoying.

Simple, because each record has a clear job.

Annoying, because after you set the records, you wait. Sometimes a few minutes. Sometimes longer. Sometimes one resolver sees the change and another one acts like nothing happened.

Codex helped me check propagation with `dig`, which was useful because I could compare what DNS was returning instead of guessing from the UI.

Eventually the records showed up, Zoho accepted the domain, and mail started working.

That was the first big win of the weekend.

At that point I had:

- A domain.
- Working email.
- DNS records that made sense.

The next step was making the domain point to actual web pages.

That is part 3.
