---
title: "The Craft Is Not Writing Code"
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, tokens, code-quality, craft, opinion]
description: "Paying an AI per token is the lines-of-code mistake all over again — the real point is what the craft of engineering pays for."
draft: true
---

> **TL;DR**
>
> - A YouTube comment: paying AI per token is like paying programmers per line of code. You reward the behavior you don't want.
> - The mistake is old — Dijkstra called out lines-of-code in 1988. The new part is the machine that produces the lines at scale.
> - A good programmer's asset is the design they can hold in their head. Agents optimize for producing code; the craft is about not producing it.

A YouTube comment said what I had been circling. Paying an AI per token is like paying a programmer per line of code: you reinforce the behavior you don't want. The commenter had worked at Google, where plenty of engineers treated every line as a long-term liability, and some prided themselves on having deleted more lines than they wrote.

The comment is right, and it stops one step early. It treats the token invoice as the problem. The invoice is where the problem shows up. The problem is what we think the work is.

A good programmer is not someone who writes code. A good programmer invents more efficient ways to do the thing — modular, clever, maintainable — and when something breaks, they know where to look, because they can hold the design in their head. That is the asset being paid for, and it is the asset AI-generated code dissolves. The more complex the output, the harder it is for anyone to repair, including the agent that wrote it.

There is a meme for this, and I keep applying it to Claude. A [2016 tweet by @jlevicoy](https://x.com/jlevicoy/status/685624118436347905), in Spanish: "Cuando escribí este código, solo Dios y yo sabíamos cómo funcionaba. Ahora solo Dios lo sabe." Only God knows. The joke is old — an English version has circulated since the 1990s — but the agent made it literal. An assistant optimizes for producing code. The craft is about not producing it.

[Dijkstra made the same point in 1988](https://www.cs.utexas.edu/~EWD/transcriptions/EWD10xx/EWD1036.html). If we must count lines of code, count them as lines *spent*, not lines *produced*, and book them on the wrong side of the ledger at your peril. Measuring productivity by lines produced per month, he wrote, is "a very costly measuring unit, because it encourages the writing of insipid code."

The commenter did not know they were paraphrasing him. The mistake is not new, and neither is the remedy. What is new is the machine that produces the lines at scale, and the invoice that bills by the token — output tokens priced at four to five times the input. Vendor revenue scales with words emitted, not work completed.

The cost of a line of code shows up after you write it, when someone has to read it. A [field study put professional developers](https://dl.acm.org/doi/10.1145/3180155.3182538) at about 58% of their time on comprehension — understanding code, not writing it. Every new line you add is a line someone has to debug and support, in [Jeff Atwood's 2007 phrasing](https://blog.codinghorror.com/the-best-code-is-no-code-at-all/). So the strongest evidence of craft is the line you deleted.

There is already a counter-example worth naming. [Ponytail](https://github.com/DietrichGebert/ponytail) is a small open-source skill that makes an agent "think like the laziest senior dev in the room — the best code is the code you never wrote." It encodes the craft as a ladder the agent has to stop on:

1. Does this need to exist? No: skip it.
2. Already in this codebase? Reuse it.
3. Does the stdlib do it? Use it.
4. Native platform feature? Use it.
5. Installed dependency? Use it.
6. One line? One line.
7. Only then: the minimum that works.

Its framing fixes Dijkstra's direction: lazy about the solution, never about reading. Validation, data-loss handling, security and accessibility never get cut. Run against the same agent with no skill, on a twelve-ticket FastAPI template, it reports 54% fewer lines, 22% fewer tokens, 20% lower cost, 27% less time, and the same safety score. The repo also documents that an earlier "80–94% less code" figure was a single-shot artifact, and corrects it in public. That correction is the honest part.

One result lands close to home. A "terse prose" arm — the caveman mode I run my own agent in — cut lines by 20% but spent 7% more tokens. Being terse does not save tokens. Writing less code does. The invoice does not care how few words you use; it cares how much code you emit.

Pay for features, the commenter said, and subtract for bugs and dead lines. I doubt the invoice ever works that way. But the craft already does. The best code is the code you never wrote, and the agent that learns to write less is worth more than the one that writes more — whatever the token price.
