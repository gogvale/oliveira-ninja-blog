---
title: "You Will Own Nothing and You Will Be Happy"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, vibe-coding, astro, dashboard, automation, llm, devops, project-writeup]
description: "A rant about slop: the dashboard I vibe-coded for executives, the rules that were supposed to fix it, the burnout it caused, and the line between renting code and owning it."
draft: true
image: /assets/img/posts/2026-08-31-you-will-own-nothing.png
---

> **TL;DR** — I maintain an executive dashboard where the hard data is clean and the subjective parts are a mess. The fix was supposed to be rules. The rules became markdown widgets. The widgets became code nobody comprehends — not me, not the AI that wrote it.
>
> - Cold data is easy. Subjective judgment is not, even with detailed rules.
> - Vibe-coded software is rented, not owned. The rent comes due when requirements change.
> - The expectation gap — AI speed demanded, traditional quality expected — is what burns people out.
> - The line is the lifecycle: vibe the things nobody will depend on in six months; engineer the rest.

There is a meme that says you will own nothing and you will be happy. I have a new interpretation: vibe-code a dashboard, then try to maintain it. That is what owning nothing feels like.

## The tweet that started it

The term "vibe coding" comes from a [single tweet by Andrej Karpathy on February 2, 2025](https://x.com/karpathy/status/1886192184808149383) — millions of views, and within a year [Collins Dictionary's Word of the Year](https://www.questera.ai/blogs/history-of-vibe-coding-karpathy-tweet). His description was inviting: fully give in to the vibes, embrace exponentials, forget that the code even exists.

![Tweet by Andrej Karpathy coining "vibe coding"](/assets/img/posts/2026-08-31-karpathy-vibe-coding-tweet.png){: width="520" }
*Andrej Karpathy's original tweet — source: [x.com/karpathy/status/1886192184808149383](https://x.com/karpathy/status/1886192184808149383)*

The invitation works. The fine print does not. "Forget that the code even exists" is the promise and the trap — because someone eventually has to *remember* it. And note what Karpathy himself said about it in that same tweet: *"It's not too bad for throwaway weekend projects."* He knew. The caveat was in the original announcement — the people who adopted it as a methodology skipped the caveat.

## The dashboard that ate my week

The project is an executive dashboard. The cold data is easy — it comes from Monday and Airtable, structured, reused in other places, boring in the best way. The problem is the subjective layer. Executives want judgment, not just numbers: what is going well, what needs attention, what the numbers do not say. I handed that to the AI with a very detailed set of rules. This is the thing AI is supposed to be good at.

It was not.

## The rules that were supposed to save it

The complaints came fast: too much information, too much noise. So we defined a plan. Part of the content would be human-written. Part would be AI-assumed with a grain of salt — a summary layer, clearly labeled as such. Part would be painted by a new rule, so the tone matched what the executives wanted.

Sound like a plan? It was. On paper.

The project got migrated to Astro. Each widget became a markdown file. The idea was auditability: read the markdown, see what the widget is supposed to do, fix it. The slop became *visible*. That was the first illusion: seeing the mess is not the same as fixing the mess.

## The code nobody comprehends

Here is where it gets embarrassing. The new rules do not stick. The AI struggles to apply them consistently. Every fix introduces a new inconsistency somewhere else. And I am stuck staring at code I did not write, did not understand while it was being written, and cannot reason about now.

The AI that generated it cannot fix it either. It tries, confidently, and produces another layer of plausible confusion. The model does not *own* the code. It generated it from a context that has already drifted. Every conversation is a new model with amnesia, re-deriving the rules from scratch, half-remembering the last thousand tokens.

> "Vibe-coded software is rented, not owned. The model forgets, and you are left with the mortgage."

This is not a personal failure. It has a name — **comprehension debt**: the code outpaces the human's ability to internalize it, and unlike old messy code, [AI-generated code looks brand new, with no signal that comprehension has lagged](https://diptendud.medium.com/comprehension-debt-and-if-youre-shipping-ai-generated-code-in-2026-and-haven-t-heard-the-term-a9b33d2f4b66). The data agrees. [GitClear's analysis of 211 million changed lines](https://www.gitclear.com/blog/gitclear_ai_code_quality_research_pre_release) found roughly 41% of new code is AI-generated, near-term rework doubled from ~3% to ~5.7%, and copy-paste code grows about eight times faster than refactored code. Every fix I make that introduces a new inconsistency is the pattern, not the exception.

## The psychology of the deadline

Here is the part nobody puts in the tweet. The executives do not care about comprehension debt. They see a tool that writes code at the speed of conversation, and they expect results at that speed — with the same quality as the old, slow, human-written way. They are not unreasonable. They are following [the logic Wharton calls the AI efficiency trap](https://knowledge.wharton.upenn.edu/article/the-ai-efficiency-trap-when-productivity-tools-create-perpetual-pressure/): if technology delivers more in less time, the rational response is to ask for more deliverables.

The problem is that the speed they see is the *generation* speed. The quality they demand is the *maintenance* quality. Those run on different clocks. [A randomized trial by METR found experienced developers were 19% slower with AI tools on real tasks — while believing they were 20% faster](https://letsdatascience.com/blog/developers-thought-ai-made-them-faster-the-data-said-otherwise). The gap between perception and reality gets absorbed by the developer: feel fast, get told to go faster, carry higher [cognitive load reviewing unfamiliar AI patterns](https://www.askflux.ai/blog/under-pressure-engineering-in-the-age-of-ai), and deliver quality in a system nobody comprehends.

That is the burnout recipe. [Bloomberg reported in 2026 that AI anxiety is fueling burnout across Silicon Valley](https://www.bloomberg.com/news/articles/2026-06-26/ai-anxiety-is-fueling-burnout-across-silicon-valley-s-tech-workers) — "Claude Code is cutting into sleep time." [A UC Berkeley Labor Center study found 67% of workers who adopted AI in 2025 reported working more hours, not fewer, by year end](https://www.aimagicx.com/blog/ai-productivity-paradox-exhaustion-burnout-2026). The tool of liberation became the tool of extension. The human becomes the buffer between the AI's promises and the organization's demands. The buffer wears out.

## The line: lifecycle, not tool

So what is the honest conclusion? It is not "never use AI." It is not "AI is fine, stop complaining." The line is the lifecycle — and the industry has converged on it:

- Vibe coding wins where [the cost of failure is low](https://techvedhas.com/is-vibe-coding-enough-for-production-software/): prototypes, demos, throwaway scripts, exploration.
- It breaks on [code someone else will maintain](https://sureprompts.com/blog/vibe-coding-the-complete-guide-2026): production, anything touching money or auth, anything with a lifecycle beyond next month.
- The decision framework is simple: [spec-driven when the system is complex, has many stakeholders, or will be maintained long-term](https://particle41.com/insights/is-vibe-coding-the-future-or-dangerous/); vibe when the cost of specs exceeds the cost of rework.

One question decides it: **will anyone still depend on this in six months?**

- No → vibe it. Prototype, demo, abandonware. Rent it freely, throw it away. That is what the tool is for.
- Yes → engineer it. Someone will consume it periodically, which means someone will maintain it, which means comprehension debt accrues. The [three-month wall](https://www.augmentcode.com/guides/vibe-coding-vs-spec-driven-development) arrives whether you believe in it or not.

My dashboard crossed the line without asking me. It started as a tool and became an obligation — the moment executives started consuming it periodically, the rental agreement changed. The rent came due the day the requirements moved.

## The grain of salt, applied to my own process

We built a grain-of-salt layer for the executives: here is the number, here is the assumption, do not treat this as gospel. I should have built one for myself. Every widget the AI generated should have been tagged: *generated by AI, understood by no one, treat with suspicion*.

The cold data was never the problem. The judgment layer was — because judgment requires context, and context is exactly what the amnesiac model cannot hold.

I do not know if this dashboard gets fixed. The most honest outcome might be to rebuild the subjective widgets from scratch, by hand, slowly, and let the AI write only the boring parts — the structured fetch, the formatted table, the thing that never changes. The thing I can actually own.

You will own nothing and you will be happy — unless you build something people keep. Then you own everything, including the debt. And you will be the one who has to understand it.

---

*Draft — saved for later. Not for publication yet. To do at publish time: verify the tweet screenshot renders, generate the banner, and check the LinkedIn caption lands the meme twist.*
