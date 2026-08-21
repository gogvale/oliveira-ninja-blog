---
title: "Let the Smart Model Plan. Let the Cheap Model Build."
date: 2026-08-21 12:35:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, claude, haiku, agents, automation, llm, devops, project-writeup, weekend-project]
description: "How I discovered that a cheap model with a good plan beats a smart model with no plan — for speed, cost, and fewer headaches."
image: /assets/img/posts/2026-08-21-smart-plan-cheap-build.png
---

My Claude credits ran out this week. The good model was locked until the next billing cycle, so I poked around the free tier and found Haiku 4.6 sitting there, waiting.

I expected a downgrade. What I got was a surprise: for the automation work I do most days, the "dumb" model executed tasks better than the smart one, with less headache.

This post is about that surprise, and about the habit it started.

## The discovery

Most of what I do with an AI agent is not deep reasoning. It is: understand my intention, then go execute — write the script, lay out the folder, wire the skill, run the thing, fix what breaks.

I had been feeding all of that to the most capable model available, because that is what you do. Then the credit wall forced a change, and the small model handled the execution side of those tasks without drama.

It was not magic. The small model was just working with a plan that already existed. The intention had been shaped, the structure decided, the edge cases mapped. What remained was the boring, mechanical part, and the cheap model does that part fast.

## The split that clicked

The realization was simple: the models are good at different things, and the split is not about intelligence. It is about the shape of the work.

- A smart model is good at turning a vague intention into a plan: the scripts to write, the structure to create, the skills to define, the order of operations.
- A cheap model is good at executing a plan that already exists: following the structure, writing the code, finishing the task, doing it at speed.

One does the thinking so the other does not have to. The result is faster, more predictable, and cheaper by an order of magnitude. I spend the expensive tokens on the small part that needs judgment, and the cheap tokens on the large part that needs execution.

## This is a documented pattern

This split has a name and a body of practice behind it. Anthropic's own guidance on [building effective agents](https://www.anthropic.com/engineering/building-effective-agents) describes an **orchestrator-workers workflow**: a central model breaks the task down, delegates the pieces to worker models, and synthesizes the results. That is exactly the shape of what I stumbled into — one model planning, cheaper models executing.

Anthropic also positioned Haiku for agentic work from the start. The [launch post for Haiku 4.5](https://www.anthropic.com/news/claude-haiku-4-5) called out sub-agent orchestration and tool-heavy tasks as its home turf, which matches the free-model experience I had.

The numbers back the instinct. [Qodo benchmarked Haiku 4.5 against Sonnet 4.5 on 400 real pull requests](https://www.qodo.ai/blog/thinking-vs-thinking-benchmarking-claude-haiku-4-5-and-sonnet-4-5-on-400-real-prs/) and found the small model competitive on the mechanical parts of code review at a fraction of the cost.

The model-routing crowd has been making the same argument for years: [cascade cheap models first](https://tianpan.co/blog/2025-11-03-llm-routing-model-cascades), escalate to the expensive one only when the task actually needs it. That pattern keeps most of the quality while cutting most of the bill — 45 to 85 percent savings, with 95 percent of the quality retained.

Even the community noticed the same thing I did. There is a popular [thread in the Claude Code subreddit](https://www.reddit.com/r/ClaudeCode/comments/1o9to45/haiku_45_is_surprisingly_good_at_writing_code_if/) titled, essentially, "Haiku is surprisingly good at writing code — if there is a plan." The plan is the missing ingredient. Without one, the cheap model flails. With one, it flies.

## What changed

I no longer default everything to the biggest model. The workflow is now: **use the smart model to shape the plan, then hand the execution to the cheap one**. The expensive model does less, the cheap model does more, and the overall result is faster and cheaper than either alone.

The funny part is my own setup already worked this way. The agent that helps with this blog, the skills that encode the structure, the scripts that do the repetitive work — those are the plan. The model executing them is the worker. I just did not see the pattern until the credit wall made me look.

The lesson stuck: for automation, the model is not the bottleneck. The plan is. Spend the expensive tokens where judgment lives, and let the cheap tokens do the walking.

That is a pretty good trade.
