---
title: "I Let the Cheap Model Drive"
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, claude-code, deepseek, svelte, cost]
description: "After 800 million tokens fighting Terra and Luna, I pointed Claude Code at DeepSeek Flash. One hour, one repo, and a $0.17 invoice."
draft: true
---

> **TL;DR**
>
> - Six days ago I closed a post with a test I had not run: let the cheap tier program without me narrating every step. I ran it.
> - Claude Code with a billed DeepSeek Flash key, on a Fedora laptop that had been closed for months. Caveman and ponytail went in before the first prompt.
> - One hour later: a scaffolded repo, stories with dates, a handoff note, and a $0.17 invoice.

Six days ago I [ended a post](https://blog.oliveira.ninja/posts/terra-costs-me-more-than-sol/) with a test I had not run: let the cheap tier program without me narrating every step. This morning I ran it.

## A smaller test

The last attempt was 800 million tokens and a week of arguing with Terra and Luna before I got something I could keep. Part of that was the shape of the request: I asked for a whole application in one breath. Part of it was the models returning half a job with a full report. I wanted a smaller test this time: one project, one repo, and enough constraints that the model could see the whole job.

At work I already use Claude Sonnet with `gh` to automate reports. That is scripting, not development. For development I reach for Bob, though I have not touched it in months. Claude Code as a build tool was new to me, and the Fedora laptop had been closed since the last time I needed a Linux machine.

Installing the CLI and the keys took minutes. Before the first prompt I installed caveman and [ponytail](https://github.com/DietrichGebert/ponytail). Caveman cuts the agent's prose to the bone. Ponytail makes it walk down a ladder before writing code: does this need to exist, is it already in the repo, does the standard library do it. I wanted fewer tokens spent explaining and fewer lines spent on code that only exists because the assistant felt helpful: the kind of code that arrives with an extra layer of abstraction and a comment explaining it.

## The list

Then I gave it the list I would give a senior dev on a Rails team: tests first, schemas at the edges, convention over configuration, types everywhere and no `any`, a small dependency tree, libraries with recent releases and no open advisories, memory that stays flat, and a few rules for how I want sessions to run. One rule sat above the rest: a change should cost one line, or none.

The useful part of Rails is the conventions: decide once, reuse it. I wanted that for a TypeScript project, which is a harder ask. Most of the ecosystem answers a missing convention with another dependency.

I did not want a framework that charges tokens for every file it touches. I suspected Svelte because it compiles most of itself away. It came back with SvelteKit, which is Svelte plus the routing and build conventions I was asking for.

We spent the next while arguing about libraries. While we argued, it installed the candidates in a local demo repo and ran the audits. Compatibility and advisories stopped being opinions. A library had to survive the demo repo before it made it into the real one.

The session was setup, not a feature. It set up the repo and the GitHub project, updated the stories and their dates, and pushed the first code. Then it left a note for the next session: what was decided, what was still open, where to start. That note is the part I did not expect to value. A repo you can reopen without reconstructing the argument is a different asset than a folder of code.

## The invoice

One hour. The invoice: $0.17. Billed, not a free tier, so the number is real.

I am not going to put the two numbers side by side. The old number bought a different job, and I did not measure the same thing. What I can compare is the shape of the hour. I spent it deciding what the project should be, not correcting a model that would not read the file it was editing.

The cheap model did not need a babysitter. It read the files, argued about a couple of choices, and left the repo in a state I could pick up. I am not going to claim it is smarter than Sol. That is not the test. The test was whether I could leave it alone for an hour with a job small enough to hold.

The difference was the frame. Two skills, one repo, a list of libraries to audit, and a rule for what a change may cost. When the job has edges, a cheaper model can see all of them. When the job is "build me an app," it fills the empty space with code and the invoice follows.

## The other key

While I write this, a second key is doing the same thing from my phone: researching comparable products and drafting a wireframe I will edit. Same setup, different job. Six days ago I had one frustrated week and a large bill. Today I have a repo I can reopen and a second session running from my phone.

The project is still complex. The session was small: one repo, two skills, a list of rules, and a place to stop. The bill was $0.17.
