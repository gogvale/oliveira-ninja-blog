---
title: "I Let the Cheap Model Drive"
date: 2026-11-07 00:30:00 -0600
categories: [AI]
tags: [ai-lab, claude-code, deepseek, svelte, cost]
image: /assets/img/posts/cheap-model-drive.png
description: "A week of PHP2Shell asset hunting left me wanting to automate it. I handed the first slice to DeepSeek Flash through Claude Code: one hour, $0.17, no handholding."
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/cheap-model-drive.mp3">Your browser does not support the audio element.</audio>

> **TL;DR**
>
> - Last week I wrote about Luna and Terra being lazy and Sol being expensive. This was my test of the other direction.
> - After a week of hunting PHP servers for PHP2Shell, I wanted a tool that did the hunting. I dusted off an old PC and put Claude Code behind a new DeepSeek Flash key.
> - Under an hour and $0.17 later: private repo, private project, ordered tasks, SvelteKit boilerplate, MIT license, docs. No handholding, and I stayed in control.

Last week I [wrote about](https://blog.oliveira.ninja/posts/terra-costs-me-more-than-sol/) how frustrating it was to run OpenAI models. Luna and Terra are "lazy" and produce sub-par results, while Sol gives what I want without being explicit but comes at a great cost. Today I decided to try something different and use DeepSeek Flash to code, with Claude Code CLI as the harness.

## The idea

It all started with an idea for a new project: given a CVE, hunt for affected assets. The idea is not mine, but I lived the experience in my previous project, with all the supply chain attacks that keep coming since the end of last year. Not all organizations have, or are willing to maintain, an SBOM, so I had to create one myself and, with a skill, keep monitoring daily whether a newly infected library affected one of our client's projects. Delegating the task was a pain. Thankfully AI is here to help with actually running the code in natural language, but that's a story for another day.

## The week PHP2Shell landed

With [PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce) everyone was on high alert. The issue was finding servers using PHP: lots of shadow IT, demo projects, and instances dangerously near actual data, so we were asked to give it maximum priority. My whole week went to tracking down assets, vulnerabilities, hints, and owners, and starting to track who was responding. I wish that job were easier.

Suddenly an idea came to me: why not automate it? Getting actual data access to the sources is in itself a challenge. It is confidential information, and most security teams require a lengthy process in order to receive limited access, with an expiration date. What if we treat it as a prototype?

I really want to get into more details, but let's leave that for another post. What I wanted to focus on here is DeepSeek's capability for the task.

## The test

I like to use Claude as a companion automating reports, investigation, throwaway scripts, and skills to save time at work. It's been a long time since I actually wrote a full programming project from zero. I use DeepSeek with Hermes on Telegram for fun and investigation, and I had heard horror stories about its ability to write code.

I decided to dust off my old PC and install the Claude Code CLI with a new DeepSeek API key, then use it for vibe coding. I wanted to save tokens and keep the work structured, closer to spec-driven development but in an agile way. I investigated historical data with Hermes, then asked it to create a private repo and a private project on GitHub, and to create multiple tasks in the order I like to work: scripts, tests, database schemas, UI, more tests, all using the best practices from experienced Rails developers. I expected hallucinations. So far, so good.

## The hour

Then came the coding: SvelteKit, IndexedDB libraries, and investigating available libraries for token saving, programmer happiness, maintainability, UX, UI speed, minimal footprint, and maintained libraries. It found everything, and tested locally with a dummy project to make sure it compiled. I didn't have to handhold it on every step, so it was very similar to my experience with frontier models.

MIT license, project task updated, dates fixed, markdown files updated, boilerplate created. All of that in under an hour, for $0.17.

I was euphoric. Not only did it behave like I wanted, it felt like an actual assistant, and I felt in control.

## The wireframe

Next step was closing the laptop, switching back to Telegram, and asking Hermes to investigate the UI for similar products and create a low-fidelity wireframe in Excalidraw, so we have a draft to get started on the actual look and feel of the application. In my opinion, having the happy path and seeing how the data is handled helps a lot when making database schema decisions.

I really enjoyed the experience. Let's hope the momentum keeps going, and I'll keep posting updates with my progress.
