---
title: "I Turned a HAR File Into a Coworker"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Security]
tags: [ai-lab, security, automation, har, burp]
draft: true
description: "A certificate policy broke my proxy the week PHP2Shell shipped. Saving the Network tab as a HAR and handing it to Claude did the work in five minutes."
---

> **TL;DR**
>
> - A policy change on my laptop required certificates for anything inspecting traffic, and Burp Suite stopped talking to the browser's proxy.
> - I stopped fixing the proxy and saved the Network tab as a HAR. A script over that file answered questions faster than clicking through Burp's history.
> - The week PHP2Shell landed I used the same file to turn a console with no export button into a name-to-email table, in under five minutes.

A policy change landed on my laptop: a certificate for anything that inspects traffic. Burp Suite stopped working against the browser's proxy. Not routing, not TLS, not the certificate. Layer 8.

I could have spent the afternoon on it. I had work to do, so I opened the Network tab instead.

The browser already records every request the page makes: method, URL, headers, response body, status, timing. Right-click, save all as HAR. That file is JSON, which means it goes through `jq`, through a script, through a diff, and into a fixture if I want it next month. Burp is a probe — you send one thing and read the answer. The HAR is a corpus — everything the page already did, in one file. Searching a corpus with a script beats searching it with my eyes, and that was the first surprise: I was not making do, I was faster.

Then the CVE week arrived.

[PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce) is two bugs in the PHP engine, CVE-2026-109618 and CVE-2026-109619, both CVSS 9.2, fixed in 8.5.12, 8.4.27, 8.3.36 and 8.2.35. The [PHP security advisory](https://www.php.net/archive/2026.php) calls them "two critical vulnerabilities that can allow unauthenticated remote code execution" and puts the highest risk on mod_php, where a request can trigger the attack before any application code runs. [watchTowr](https://watchtowr.com/intelligence/php-cve-2026-109618-faq/) saw exploitation in the wild the day the patches shipped. [Sansec](https://sansec.io/research/php2shell) was calmer: the public exploits do not work against a default install, because the attack needs to know things about the target. That detail made for a better blog post than it did for a decision, because my problem was never patching PHP. My problem was answering whether we were exposed, using sources I could only read in a browser.

The person holding the widest access was out until the following week. So there was no export button, no API key, and no one to ask. There was a search box.

I opened the ticket console, kept the Network tab open, searched one user, and found the request that carried the email back. Saved the HAR. Then I gave Claude the file and a list of names and asked it to map one to the other. It did, in under five minutes, with the console as the only system touched.

Bad practice, and I knew it while I was doing it. That HAR came from an authenticated session, so the cookies and the token came with it. If this were a pipeline and not an afternoon, I would strip the auth headers first, or replay a single call with a token scoped to reading one field. I was testing what the shape of the work allows. I was not writing a runbook, and this is not one.

The part I did not plan: the out-of-office replies. Once the names had addresses, the auto-responders started arriving with the dates, the coverage, and the person covering. A second pass over the same file found the backups. An afternoon of clicking turned into a table I could sort, and the table answered the question that had started the week.

There is a symmetry here I did not go looking for. The bug itself came out of the same shift: Wiz says demonstrating exploitability "required an exceptionally complex chain of steps, at the edge of what human researchers could practically achieve", and that "frontier AI models substantially reduced that barrier". One week, one model doing the hardest part of a decade-old vulnerability, and the same model reading a JSON file so I could find who was on call.

The durable piece is small. An interface that will not export you is still an interface. Your browser ships with a recorder, and whatever the page did, it did over HTTP, and it is in the file.

I still keep Burp installed. It is the right tool when I want to change a request and watch what happens. But when a tool cannot see traffic, the browser will hand me the traffic as a file, and a file is something a script can hold.
