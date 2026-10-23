---
title: "I Turned a HAR File Into a Coworker"
date: 2026-10-23 09:30:00 -0600
categories: [AI, Security]
tags: [ai-lab, security, automation, har, burp]
image: /assets/img/posts/har-file-coworker.png
description: "A certificate policy broke my proxy the week PHP2Shell shipped. Saving the Network tab as a HAR and handing it to Claude did the work in five minutes."
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/har-file-coworker.mp3">Your browser does not support the audio element.</audio>

> **TL;DR**
>
> - A policy change on my laptop required certificates for anything inspecting traffic, and Burp Suite stopped talking to the browser's proxy.
> - I stopped fixing the proxy and saved the Network tab as a HAR. A script over that file answered questions faster than clicking through Burp's history.
> - The week PHP2Shell landed I used the same file to turn a console with no export button into a name-to-email table, in under five minutes.

A policy change landed on my laptop: a certificate for anything that inspects traffic. Burp Suite stopped working against the browser's proxy the same week. I never found out whether the policy did it or I did.

I could have spent the afternoon on it. I had work to do, so I opened the Network tab instead.

The browser already keeps a record of every request the page makes. Right-click in the Network tab, save all as HAR. It is a plain file in a format a program can read: a parser, a script, an agent. Burp is a probe — you send one request and read the answer. The HAR is the whole conversation the page already had, saved. Searching a file with a script beat searching it with my eyes, and that was the first surprise: I was not making do, I was faster.

The week PHP2Shell landed, the clock was the problem. Two bugs in the PHP engine, CVSS 9.2, patched on October 21 and seen in the wild the same day ([details](https://www.wiz.io/blog/php2shell-php-rce), [exploitation](https://watchtowr.com/intelligence/php-cve-2026-109618-faq/)). What matters here is what that week did to my afternoon: I had days, not weeks, to answer whether we were exposed, out of sources I could only read in a browser.

The person holding the widest access was out until the following week. So there was no export button, no API key, and no one to ask. There was a search box.

I opened the ticket console, kept the Network tab open, searched one user, and found the request that carried the email back. Saved the HAR. Then I gave Claude the file and a list of names and asked it to map one to the other. It did, in under five minutes, with the console as the only system touched.

Bad practice, and I knew it while I was doing it. That HAR came from an authenticated session, so the cookies and the token came with it. Set it next to the RPA this replaces: a script with a password pasted in at the top, running on a service account that never expires. Mine is a session cookie that dies in an hour, and when it dies the HAR goes with it. Expiry is not a security control, but it beats the hardcoded credential that already runs in production.

There is a symmetry here I did not go looking for. The bug itself came out of the same shift: Wiz says demonstrating exploitability "required an exceptionally complex chain of steps, at the edge of what human researchers could practically achieve", and that "frontier AI models substantially reduced that barrier". One week, one model doing the hardest part of a decade-old vulnerability, and the same model reading a JSON file so I could find who was on call.

The durable piece is small. An interface that will not export you is still an interface. Your browser ships with a recorder, and whatever the page did, it did over HTTP, and it is in the file.

Burp still does not talk to the browser's proxy on that laptop. I have not gone back to fix it, because the file does more than the proxy did: it outlives the tab, it survives a diff, and I can hand it to something that reads faster than I do.
