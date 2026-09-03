---
title: "Spec-Driven Development Is Dead. Long Live Vibe Coding."
date: 2026-09-01 15:05:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, sdd, spec-driven, vibe-coding, claude, agents, llm, workflow, project-writeup]
description: "The rise and quiet fall of spec-driven development — and why the same hype cycle that buried prompt engineering is repeating."
image: /assets/img/posts/2026-09-01-sdd-is-dead.png
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/2026-09-01-sdd-is-dead.mp3">Your browser does not support the audio element.</audio>
> **TL;DR** — A year ago, spec-driven development was the answer to hallucinated code: write the whole app as markdown specs, then let the agent implement. Tools like GitHub's spec-kit bet on it. Now nobody mentions it.
>
> - SDD became markdown theater — 1,300 lines of spec for a date display.
> - Models crossed the intent-understanding threshold; they no longer need specs to "understand" you.
> - It is the same arc as prompt engineering: the discipline was a bridge, not a destination.
> - The pattern will repeat. The bridge is always temporary.

A few months ago, spec-driven development was going to save us all.

The pitch was seductive: vibe coding hallucinates because the model has no constraints. So constrain it. Write the entire application as a stack of markdown files — requirements, design, tasks — then hand the stack to the agent and let it implement exactly what the spec says. No drift. No invention. Just execution.

[GitHub's spec-kit](https://jamesm.blog/ai/github-spec-kit/) led the charge. [AWS had Kiro](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html). [Tessl](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html) built a whole platform on it. Conferences, blog posts, tooling — the works. If you squinted, it even looked like the future of software engineering.

I tried it. I found it overwhelming.

## The spec that ate the feature

The pattern was always the same. A tiny feature would spawn a miniature bureaucracy. One example floating around the SDD discourse: adding a date display to a time-tracking app produced [8 spec files and 1,300 lines of markdown](https://marmelab.com/blog/2025/11/12/spec-driven-development-waterfall-strikes-back.html). For a date. On a screen.

That example is not the outlier. It is the genre.

[The marmelab takedown](https://marmelab.com/blog/2025/11/12/spec-driven-development-waterfall-strikes-back.html) named the failure modes precisely: *markdown madness* (you spend your time reading specs, not thinking), *double code review* (the spec contains code, then the implementation contains code — you review both), and *faux agile* (the toolkits generate "user stories" that are not user stories). The verdict: **"Spending 80% of your time reading instead of thinking is, in my opinion, not worth it."**

For someone coming from Agile, SDD felt like discovering waterfall by accident. We spent two decades escaping the idea that you can fully specify software before writing it. SDD smuggled it back in through the AI door, wearing a markdown trench coat.

## What changed

Then the models got better. Not gradually — [between November 2024 and March 2025, frontier models crossed a threshold](https://futurefactors.ai/prompt-engineering-dead-what-replaced-it-2026/) from "good at following instructions" to "good at understanding what you are actually trying to accomplish."

That is the whole story. The spec existed to translate intention into something the model could follow. Once the model understood intention directly, the translation layer became optional.

Now the workflow is: tell it your intention, add the baseline guardrails ("do not forget tests"), point it at the schema or data source, and let it work. The spec that took an afternoon to write is replaced by a paragraph. The 1,300 lines become a sentence.

## The same arc, second verse

This is not a new pattern. It is the prompt engineering story, rerun.

In 2023, prompt engineering was going to be the career of the future. Courses, certifications, entire job listings. Then the models crossed the same threshold, [the standalone job title effectively disappeared at companies running frontier models](https://www.solidaitech.com/2026/04/prompt-engineer-job-dead-ai-careers.html), and the craft got [absorbed into broader roles](https://dev.to/gabrielanhaia/prompt-engineering-is-mostly-dead-in-2026-heres-what-replaced-it-433b) — context engineering, evals, agent loops. The skill that remained real (production context engineering) is not what the courses sold.

The lesson is the same twice over: **when the interface between human intention and model output is a temporary gap, the elaborate bridge built across it is also temporary.** The bridge gets shorter every time the models improve. The discipline of building the bridge is real — for a while.

## The pattern will repeat

Here is the uncomfortable part. This is going to happen again.

Some discipline is being hyped right now — as a "new career" or a "methodology that fixes everything" — and in eighteen months it will be quietly absorbed into the default workflow, and nobody will mention it. The bridge builders will have moved on to the next gap.

The honest take is not "SDD was stupid." It served a real purpose for a real window: when models could not reliably hold a large intention, the spec held it for them. The same is true of prompt engineering — it was a real skill while the interface needed translation.

The trap is treating the bridge as the destination. The people who got burned by the hype treated a temporary interface problem as a permanent discipline. The people who won treated it as what it was: a way to get from here to there, knowing the terrain would shift.

> "When the interface is a temporary gap, the bridge across it is also temporary."

## What I use instead

The workflow that survived the SDD experiment is embarrassingly simple:

- **State the intention** in plain language. What is the thing, who uses it, what does success look like.
- **Add the baseline rules** in one or two lines. Do not forget tests. Keep the existing patterns. Use this schema.
- **Point at the source of truth.** The schema, the API, the data model — the thing the model must not invent.
- **Review like an engineer.** The output is a first draft from a junior with infinite confidence. Read it like one.

That is not a methodology. It is a paragraph. And it works because the model does the part that used to require translation — and the human does the part that still requires judgment.

The spec is dead. Long live the paragraph. And in a year, this post will be the bridge-builder's note to self.

---

*Draft of a bridge-builder's note: the pattern is the point, not the specific tool. When the next "fixes everything" methodology appears, remember the spec. Remember the prompt engineer. The bridge is always temporary.*
