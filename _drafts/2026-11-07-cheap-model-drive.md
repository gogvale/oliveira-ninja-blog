---
title: "The Model Wrote the Code. I Kept the Wheel."
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, claude-code, deepseek, svelte, cost]
description: "One hour on a billed DeepSeek Flash key through Claude Code: a repo, a project board, dated stories, a handoff note, and a $0.17 invoice. I spent the hour deciding."
draft: true
---

> **TL;DR**
>
> - The last attempt cost more than 800 million tokens and a week of Terra and Luna returning half a job with a full report attached.
> - This morning I put DeepSeek Flash on a billed key through Claude Code, added two skills, and wrote the rules before the first prompt.
> - One hour later: a repo, a GitHub project, dated stories, pushed code, a handoff note, and a $0.17 invoice. I spent that hour deciding.

The Fedora laptop had been closed for months. I opened it, installed Claude Code, configured a billed key for DeepSeek Flash, and put two skills in place before typing a prompt: caveman for terse replies, and [ponytail](https://github.com/DietrichGebert/ponytail) so the agent asks whether the code needs to exist before it writes any.

The install took minutes. The list of rules took longer.

I kept comparing it to the last attempt. More than 800 million tokens, a week of fighting Terra and Luna, correcting half-finished work. I wanted an hour on the other side of that loop, deciding instead of correcting.

For development I reach for Bob, though I have not opened it in months. At work I run Claude Sonnet with `gh` to automate reports, which is scripting. Claude Code as a build tool was new to me.

## Rules before prompts

The list came first. I wrote what I would hand a senior dev on a Rails team: tests before features, schemas at the edges, convention over configuration, types everywhere with no `any`, a small dependency tree, libraries with recent releases and no open advisories, memory that stays flat, and a few rules for how a session should run.

The line that shaped the rest: a change should cost one line, or none. That one is about me as much as the model. A framework that spreads every feature across five files makes the diff unreadable, and an unreadable diff is where I lose the thread and start trusting the report instead of the code.

Rails earns its keep with conventions you decide once and reuse. I wanted that in TypeScript, where the usual answer to a missing convention is another dependency. I also did not want a framework that bills me for every file it touches. Svelte compiles most of itself away, so I suspected it. The agent came back with SvelteKit, which adds the routing and build conventions I was asking for.

## The audits settled it

We argued about libraries for a while. It proposed, I pushed back, and it installed each candidate in a local demo repo and ran the audits there. Compatibility and advisories stopped being opinions I had to take on faith; the demo repo had a verdict before I put anything in the real project.

That is the piece I was missing last time: a way to settle a disagreement with output instead of a paragraph of confidence.

## What the hour left behind

By the end, there was a repo, a GitHub project, dated stories, pushed code, and a note for the next session: what we decided, what stayed open, where to start. Setup, not a feature.

The note is the part I did not expect to care about. A repo I can reopen without rebuilding the argument in my head is a different asset than a folder of code.

The invoice ran $0.17. Billed endpoint, not a free tier, so that is the real price of the hour.

The hour did not feel like supervision. I spent it deciding what the project should be, then watching those decisions turn into files. I stayed in control. When the job has edges I can hold the whole thing in my head, and so can the model. When the job is "build me an app," the empty space fills with code and the invoice follows.

## The second key

A second key has been running from my phone while I wrote this: reading up on comparable products and drafting a wireframe I will edit. Same setup, different job, and I am still the one deciding when it is done.

The project is bigger than one hour, and I will write about it when there is more to show. What I have today is a repo I can pick up again, a second session already running, and the feeling of having driven the whole hour.
