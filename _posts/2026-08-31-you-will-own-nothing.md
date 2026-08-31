---
title: "You Will Own Nothing and You Will Be Happy"
date: 2026-08-31 11:20:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, vibe-coding, astro, dashboard, automation, llm, devops, project-writeup]
description: "The dashboard I vibe-coded for executives, the rent that came due, and the line between renting code and owning it."
image: /assets/img/posts/2026-08-31-you-will-own-nothing.png
---

> **TL;DR** — I moved my AI automation to agent skills, then watched it hallucinate exactly where the work was unstructured — renamed columns, stale data, missing meeting context. The fix was old-school: a Makefile, audited sources, me running the commands.
>
> - Vibe-coded software is rented, not owned. The rent comes due when requirements change.
> - The expectation gap — AI speed demanded, traditional quality expected — is what burns people out.
> - The line is the lifecycle: vibe the things nobody will depend on in six months; engineer the rest.

You will own nothing and you will be happy. I have a new interpretation: vibe-code a dashboard, then try to maintain it.

## The tweet that started it

"Vibe coding" was coined by [Andrej Karpathy in a tweet on February 2, 2025](https://x.com/karpathy/status/1886192184808149383) — within a year it was [Collins Dictionary's Word of the Year](https://www.questera.ai/blogs/history-of-vibe-coding-karpathy-tweet). Fully give in to the vibes, embrace exponentials, forget that the code even exists.

![Tweet by Andrej Karpathy coining "vibe coding"](/assets/img/posts/2026-08-31-karpathy-vibe-coding-tweet.png){: width="520" }
*Andrej Karpathy's original tweet — source: [x.com/karpathy/status/1886192184808149383](https://x.com/karpathy/status/1886192184808149383)*

The caveat was in the same tweet: *"It's not too bad for throwaway weekend projects."* Everyone who adopted it as a methodology skipped that line.

## The dashboard that ate my week

An executive dashboard. The cold data is easy — Monday and Airtable, structured, boring in the best way. The problem is the subjective layer: what is going well, what needs attention, what the numbers do not say. I handed that to the AI with a very detailed set of rules. The thing AI is supposed to be good at.

It was not.

The complaints came fast: too much information, too much noise. So we defined a plan — part human-written, part AI-assumed with a grain of salt, part painted by a new rule. Migrated to Astro. Each widget became a markdown file, for auditability. The slop became *visible*.

Seeing the mess is not the same as fixing the mess.

## The code nobody comprehends

The new rules do not stick. Every fix introduces a new inconsistency somewhere else. I am stuck staring at code I did not write and cannot reason about. The AI cannot fix it either — every conversation is a new model with amnesia, re-deriving the rules, half-remembering the last thousand tokens.

> "Vibe-coded software is rented, not owned. The model forgets, and you are left with the mortgage."

This has a name — [comprehension debt](https://diptendud.medium.com/comprehension-debt-and-if-youre-shipping-ai-generated-code-in-2026-and-haven-t-heard-the-term-a9b33d2f4b66): the code outpaces the human's ability to internalize it, and unlike old messy code, AI-generated code *looks* brand new.

![Three stages of comprehension debt: day 1 mostly human code, day 100 mostly AI code, day 1000 almost entirely AI code with comprehension lost](/assets/img/posts/2026-08-31-comprehension-debt-stages.png){: width="600" }
*Stages of comprehension debt — source: [Allstacks, Comprehension Debt: The Hidden Cost of AI-Generated Code](https://www.allstacks.com/blog/comprehension-debt-the-hidden-cost-of-ai-generated-code)*

[GitClear's analysis of 211 million changed lines](https://www.gitclear.com/blog/gitclear_ai_code_quality_research_pre_release) found near-term rework roughly doubled as AI assistance spread, and copy-paste code growing about eight times faster than refactored code. My inconsistent fixes are the pattern, not the exception.

## The psychology of the deadline

The executives see a tool that writes code at conversation speed and expect results at that speed — with the same quality as the slow, human-written way. [Wharton calls it the AI efficiency trap](https://knowledge.wharton.upenn.edu/article/the-ai-efficiency-trap-when-productivity-tools-create-perpetual-pressure/): if technology delivers more in less time, the rational response is to demand more.

The speed they see is the *generation* speed. The quality they demand is the *maintenance* quality. Those run on different clocks. [METR's randomized trial](https://letsdatascience.com/blog/developers-thought-ai-made-them-faster-the-data-said-otherwise) found developers were 19% slower with AI tools on real tasks — while believing they were 20% faster. The gap gets absorbed by the developer: feel fast, get told to go faster, carry higher cognitive load, deliver quality in a system nobody comprehends. [67% of workers who adopted AI in 2025 reported working more hours, not fewer](https://www.aimagicx.com/blog/ai-productivity-paradox-exhaustion-burnout-2026). The human becomes the buffer between the AI's promises and the organization's demands. The buffer wears out.

## The line: lifecycle, not tool

The industry converged on it: vibe coding wins where [the cost of failure is low](https://techvedhas.com/is-vibe-coding-enough-for-production-software/) — prototypes, demos, throwaway scripts. It breaks on [code someone else will maintain](https://sureprompts.com/blog/vibe-coding-the-complete-guide-2026).

One question decides it: **will anyone still depend on this in six months?**

- No → vibe it. Prototype, demo, abandonware. Rent it freely, throw it away.
- Yes → engineer it. Someone will consume it periodically, which means comprehension debt accrues.

My dashboard crossed the line without asking me. The rent came due the day the requirements moved.

You will own nothing and you will be happy — unless you build something people keep. Then you own everything, including the debt.
