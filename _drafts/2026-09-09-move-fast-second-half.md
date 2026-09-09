---
title: "The Second Half of \"Move Fast and Break Things\""
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, opinion, devops]
description: "Move fast and break things only worked because the person breaking understood what they broke. AI kept the speed and removed the understanding."
draft: true
---

<!--
PROVENANCE (strip at publish):
- Idea + stance (Gabriel, 2026-09-09): the mantra had an unspoken second half — move fast only works while fixing stays cheap, and the breaker understands the break. Facebook retired it in 2014 (scale); AI retired it for everyone (comprehension).
- Research: ~/mfab/investigacion_move_fast.md + delegation summaries 2026-09-09 (quotes verbatim verified: Zuck 2009/2012/2014, Bosworth 2020, Osmani, Charity Majors, DORA, GitClear, arXiv burnout study, LeadDev).
- Never name the employer. Personal beats stay generic. No invented scenes.
- Seed: idea-level only.
-->

> **TL;DR**
> - "Move fast and break things" had a second half nobody said out loud: it only works while the person breaking understands what they broke.
> - Facebook retired the mantra in 2014, at scale, for exactly that reason.
> - AI kept the speed and removed the understanding. The breaker is now a generator with no memory of the break.
> - The bill arrives as comprehension debt, review bottlenecks, and burnout.

In 2009, Mark Zuckerberg called it his prime directive: *"Unless you are breaking stuff, you are not moving fast enough."*

The mantra was aimed at bureaucracy. Before it, software moved through waterfalls of approvals — a change needed sign-offs, meetings, confirmations. Against that, the hacker way was a promise: build it, ship it, ask forgiveness later. Asking what people wanted never got a straight answer anyway. Shipping and breaking did.

It worked. Not because breaking is good. Because the loop was honest: when you broke something, the feedback arrived fast, and you understood the thing you broke. You wrote it. You knew where the break was. You fixed it, and the fixing was cheap.

The people who coined the mantra killed it themselves.

## The second half

In 2014, Facebook changed the slogan to "move fast with stable infrastructure." Zuckerberg, at the developer conference, explained why:

> "When you build something that you don't have to fix 10 times, you can move forward on top of what you've built."

Read that again. The founder of the move-fast religion admitting the unspoken clause: **moving fast only works while fixing stays cheap.** At Facebook's scale, breaking things meant breaking the platform that thousands of other products stood on. Nobody could fix it ten times. So the speed had to come with stability instead.

The clause was always there. It just took five years and a public infrastructure to surface it.

## AI kept the speed and removed the understanding

The second half of the deal was that the breaker understood the break. That is the part that does not survive contact with AI.

Google's Addy Osmani named the mechanism: **comprehension debt** — the growing gap between how much code exists in a system and how much of it any human genuinely understands. His version of the flip is the whole story in one sentence:

> "A junior engineer can now generate code faster than a senior engineer can critically audit it."

The bottleneck moved. It used to sit at writing — typing code was the slow part, and understanding came along as a byproduct. Now generation is instant and understanding costs more than ever. The scarce resource is no longer authorship. It is acceptance: a human saying, with a clear conscience, that they understand what is being merged.

A practitioner I respect reviewed the same feature built by hand and by an agent. The agent wrote six times more code. The review took four times longer — not because the code was wrong, but because the question changed from "does this work?" to "is any of this necessary?"

## What breaks stays broken

Here is the part that should worry everyone who measures velocity and calls it progress.

Osmani's work cites a study where engineers using AI assistants showed a measurable drop in code comprehension. GitClear's analysis of over two hundred million changed lines found something that has never happened before in tracked software history: copied and pasted code now outnumbers code that was deliberately reused. DORA, the most-cited research program in the industry, says AI amplifies — it magnifies the strengths of high performers and the dysfunctions of struggling ones. Their summary is brutal: AI will simply help an organization generate technical debt faster.

Move fast and break things assumed the breaks got fixed. What breaks now stays broken, because the person who could fix it does not understand it, and the person who could understand it is too busy reviewing the next batch.

## The bill arrives in people

The missing feedback loop does not only cost money. It costs the people in the loop.

An academic study of developers using generative AI found the adoption itself raises job demands — and with them, burnout. Industry surveys agree: a fifth of engineering leaders and developers report critical levels of burnout. The mechanism is not mysterious. Writing got cheaper, so the organization expects more of it. Reading did not get cheaper — it got more important. Human cognition does not scale at machine speed, and someone has to absorb the difference.

Before AI, when I broke something, people were fast to correct me, and I fixed it fast. That was the forgiveness part of asking forgiveness instead of permission — the feedback made the forgiveness cheap. The loop is gone. The speed stayed, and the understanding left with the loop.

## The clause

The mantra never was "break things." It was "break what you can fix."

Facebook learned the clause at scale and retired the slogan in 2014. The rest of the industry is learning it now, the hard way, with an assistant that generates faster than anyone can audit and remembers nothing of what it broke.

> Move fast and break things only works if the person breaking can fix what they broke. AI removed the fixer. We kept the speed.

The fix is not to slow down. It is to put the understanding back in the loop: review with real attention, cap the batch sizes, make the generator explain itself. The second half of the deal is the part worth keeping — and it is the part nobody automated.
