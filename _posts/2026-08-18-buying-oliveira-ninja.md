---
title: "oliveira.ninja: The Domain Hunt"
date: 2026-08-18 00:10:00 -0600
categories: [Personal Site, Domain]
tags: [domain, dns, email, weekend-project]
description: "How I picked oliveira.ninja, compared registrars, and started turning a domain idea into a small web home."
---

I spent part of the weekend setting up something I had wanted for a while: a personal domain, email, and a small web presence. Nothing too fancy at first. Just the kind of project where you start with "I'll buy a domain real quick" and somehow end up comparing TLDs, reading DNS docs, and refreshing `dig` like it owes you money.

The flow was roughly: find a domain, buy it somewhere reasonably priced, set up email, configure DNS, wait for things to propagate, then point GitHub Pages at it. Simple list. Surprisingly fun rabbit hole.

This first part is about the domain hunt.

## The domain search

The obvious idea was some version of my name. That immediately ran into the normal domain problem: the clean options are either taken, expensive, or both.

I checked the usual suspects first:

- `.com` was taken and expensive.
- `.net` was not much better.
- `.io` looked nice, but the price did not.
- `.xyz` was also not the cheap playground I expected it to be.

So I started looking at more playful TLDs. Some of the fun, cheap, available ones I found were:

- `.my`
- `.day`
- `.click`
- `.ninja`

`.ninja` won because, honestly, why not?

It felt a little silly in the right way. Personal domains do not have to sound like enterprise procurement portals. I wanted something memorable, inexpensive, and mine.

## Where AI helped, and where it did not

I did try asking AI for creative domain ideas, but this was one of those cases where it did not really help. The suggestions were technically valid, but not very personal. They felt like generated options, not something I would actually want to put on a sticker or use as an email address.

Manual searching was much better.

I used Namecheap mostly as a discovery tool: type things in, try different endings, get a feel for what was available, and repeat until something clicked.

Once I had decided on `oliveira.ninja`, I used AI for a more practical task: researching where to buy it for the best price. That led me to Porkbun.

## Buying it

Porkbun had the better price, the checkout was simple, and suddenly `oliveira.ninja` was real.

That was a satisfying moment. A domain is just a record in someone else's system, but it still feels like claiming a tiny plot of internet land.

At that point I had the domain, but not much else. The next obvious step was email. If I was going to own `oliveira.ninja`, I wanted an address on it too.

So I bought Proton and started setting up the mail domain.

That is where part 2 begins.

---

[Continue to part 2: Mail, Plans, and DNS ->]({% post_url 2026-08-18-email-hosting-and-dns %})
