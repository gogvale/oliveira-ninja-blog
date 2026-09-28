---
title: "Claude Picks My First Task of the Day"
date: 2030-01-01 00:00:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, workflow, productivity, weekend-project]
description: "I always liked a day list — the dopamine does most of the work. Now Claude builds it from monday every morning: first task picked, duplicates flagged, nothing written without my yes."
draft: true
---

> **TL;DR**
>
> - I have always kept a list for the day. Knowing what comes first is what keeps me moving.
> - With four projects and tasks assigned across monday boards, making that list by hand turned into the hardest part of my morning.
> - Now I ask Claude for the day's queue. It reads monday live, picks the order, and asks before it writes anything.

You know the feeling: so many tasks on the plate that you do not know where to start.

I have had that week. What I never had was a problem with lists. I like them. Five or six lines for the day, tick them off one by one, and the dopamine carries the afternoon. That part has worked for me since I can remember. The thing that broke was not the work, it was producing the list.

## The list got harder to make than the work

For a long time the list lived in my head, and my head worked fine. Then the tasks started arriving from four projects at once, each one with its own workspace, its own columns, its own idea of what priority means.

The rows did not agree with each other. One deliverable had two dates. Half the tasks had no detail on what "done" looked like, or who was going to receive it. And the same job showed up in more than one place, because one team tracked it as their own work while another carried it in a report for someone else.

So my first hour went into deciding what the day was. Reading rows, comparing dates, working out which update mattered. That hour is what I decided to stop paying.

## What I put on top of monday

A small project that talks to monday through an MCP server, with Claude doing the reading. The build took a weekend, and most of that weekend went into the rules.

- **The rules live in a file I edit by hand.** Which boards to read, which columns hold status and dates, which values are valid, what is off limits.
- **It reads monday live every time.** The local copy of my tasks is a fallback for when the connection is down, not the source of truth. A stale task list is worse than no task list.
- **It triages in a fixed order:** blocked or at risk first, then overdue or due within a week, then anything with no status, then in progress, then the rest.
- **It asks before it writes.** Creating a task, changing a status, leaving a comment: all of it arrives as a proposal with the exact change, and waits. Nothing lands on someone else's board without my yes.
- **The scripts still work without the model.** If the MCP is down, a couple of Python scripts do the same reads and writes through the API. Nothing on the critical path depends on a model being up.

That last rule is the one I care about. The model reads messy input and asks the right question about it. It is not the thing I want holding the only copy of my task list.

## The morning ask

The line I type most often is the plainest one: *give me the queue for today*. Priority and urgency first, the rest under it, each item with the project it belongs to and what closing it takes.

What comes back is the list I used to build by hand, except it takes a few seconds and it does not miss the row I forgot to read. I get my five or six lines, I start at the top, and the afternoon takes care of itself.

The two recaps came later. Before I close the laptop I ask what moved, and on Friday I ask for the same over the week. Those are what I take into the standup — I stopped rebuilding the last three days from memory before a call — and the Friday one shows me the project I have been ignoring without noticing.

## What came along

Three things I did not plan for.

**The duplicates.** Seventeen rows across four projects, and five of them were the same work wearing a different hat. The one that made me laugh belonged to one team as their operational work and to another as a line in an executive report — same outcome, two owners, two dates, and me in the middle. The worst case was a triple. Grouping rows that describe one job is easy to describe and tedious to do, which is the shape of work that suits this setup. Seventeen rows turned into twelve real items.

**The questions.** More than once I hit a task with no detail on it and no one who remembered writing it. Instead of guessing, I bounced it off the assistant. What came back was not a plan I copied, it was the set of questions for whoever owned the task. "Do you want the report or the raw numbers?" is one line to send, and after the answer the task closes by itself.

**The notes.** Each closed task leaves a short record: who wanted it, what the real deliverable was, what "done" meant that time. Next time the same project shows up, the context is there. That turned into the process documentation I never write for myself, and it costs nothing because it gets written while I work.

## Why it holds

None of this is a system. It is a project folder, a connection to monday, and a habit of confirming before writing. Every action is reversible, and every decision comes back to me as a question first, which is why I trust it with a board my teammates also use.

The list is the part that matters. I did not need a new method — I needed the one I already had, built for me before I opened my laptop.
