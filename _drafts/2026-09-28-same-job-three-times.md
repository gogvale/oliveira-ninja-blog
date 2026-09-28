---
title: "The Same Job, Listed Three Times"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, workflow, productivity, weekend-project]
description: "Seventeen tasks across four projects, one deliverable with two deadlines, and a small Claude setup that reads the board for me and asks before it touches anything."
draft: true
---

> **TL;DR**
>
> - Seventeen assigned tasks across four projects. Five of them were duplicates, and the worst case had the same job listed three times.
> - The view that should fix this takes ages to load and cannot tell that two rows are one job.
> - So I built the small version: a Claude project with an MCP into the board. It reads live, groups what is the same, and asks before writing anything. The win was not speed, it was the pending list leaving my head.

My assigned list is a pile of things other people put there. Last week it was seventeen tasks spread across four projects, and a good chunk of them described work I was already tracking somewhere else. Two delivery dates for the same deliverable. Different groups expecting the same thing. Priorities that disagreed with each other, and no note on what "done" looked like for any of it.

I have the same board as everyone else in the company. It should be the place where this gets sorted. It is the place where it piles up.

## Five duplicates, and the reason

The count surprised me. Five of the seventeen were the same work wearing a different hat. The one that made me laugh was a job that lived in two places at once: one team tracked it as their own operational work, another carried it as a line in the executive report. Same outcome, two rows, two owners, two dates, and me in the middle of both.

The worst case was a triple. Three rows, one result, and no way to close them together. I spent more time working out which row to update than working on the thing itself. That is the part nobody counts: the tax is not the task, it is the rereading.

My Tasks is supposed to be the answer for this. It is not. It takes a lifetime to load, and it shows me the rows without telling me that two of them are the same. It sorts, it does not think.

## What I built

A small project that talks to the board through an MCP server, with Claude doing the reading. The whole build is maybe a weekend, and most of that weekend went into the rules, not the code.

- **The rules live in a file I edit by hand.** Which boards to read, which columns hold status and dates, which values are valid, what is off limits.
- **It reads live every time.** The local copy of the tasks is a fallback for when the connection is down, not the source of truth. A stale task list is worse than no task list.
- **It triages in a fixed order:** blocked or at risk first, then overdue or due within a week, then anything with no status, then in progress, then the rest.
- **It asks before it writes.** Creating a task, changing a status, leaving a comment: all of it arrives as a proposal with the exact change, and waits. Nothing lands on someone else's board without me saying yes.
- **The scripts still work without the model.** If the MCP is down, a couple of Python scripts do the same reads and writes through the API. Nothing on the critical path depends on a model being up.

That last point is the one I care about. The model reads messy input and asks the right question about it. It is not the thing I want holding the only copy of my task list.

## Where it helped

Two things, and neither is the one I expected.

The first is the duplicates. Grouping rows that describe one job is the kind of work that is easy to describe and tedious to do, so it fits. Seventeen rows turned into twelve real items, and I could close them once instead of leaving three rows open because I lost track of which one mattered.

The second is the brainstorm. More than once I hit a task with no detail on it — no steps, no acceptance criteria, nobody who remembered writing it — and instead of guessing I bounced it off the assistant. What came back was not a plan I copied. It was the set of questions I needed to ask the person who owns it. "Do you want the report or the raw numbers?" is a question I can send in one line, and after the answer the task closes by itself.

That changed the shape of my day more than the dedupe. The list is not smaller in a heroic way. It stopped living in my head. I open the thing, get the order, and work top down.

There is one more ask I make every morning, and it is the one that stuck: give me the queue for today, priority and urgency first. At the end of the day I ask for a recap of what moved, and on Friday the same for the whole week. Those recaps are what I take into the daily — I stopped rebuilding the last three days from memory before a call — and the Friday one shows me the project I have been ignoring without noticing.

## The part I did not plan

I started using it as notes. Each closed task leaves a short record: who wanted it, what the real deliverable was, what "done" meant that time. The next time the same project shows up, the context is there. It doubles as the process documentation I never write for myself, and it costs nothing because it gets written while I work.

None of this is a system. It is a project folder with rules, a connection to a board, and a habit of confirming before writing. It handles seventeen rows and four projects, which is my week, and it keeps working because every action is reversible and every decision comes back to me as a question first.

The list lost its grip on my head. That was the whole goal.
