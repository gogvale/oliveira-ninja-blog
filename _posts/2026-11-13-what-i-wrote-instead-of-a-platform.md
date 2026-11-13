---
title: "What I Wrote Instead of a Platform"
date: 2026-11-13 00:30:00 -0600
permalink: /posts/what-i-wrote-instead-of-a-platform/
categories: [AI]
tags: [ai-lab, agents, skills, vulnerabilities, opinion]
image: /assets/img/posts/what-i-wrote-instead-of-a-platform.png
description: "Thousands of open findings, a five-minute target, and the year-long build I did not do. Two skills and one task closed the queue."
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/what-i-wrote-instead-of-a-platform.mp3">Your browser does not support the audio element.</audio>

The vulnerability queue does not move on its own. At work we had thousands of open findings across a stack of applications, and the teams were not closing them at the speed they arrived. My team pulled the numbers apart and split the tickets by application and by owner, because a shared queue hides which team is behind. Then we set a target that fit in one sentence: a guide that lets a developer fix any of those findings in under five minutes. We scoped it to upgrades, since an upgrade is the part that automates clean.

The obvious build is a platform. Pull advisories from the sources, hand the scanner output to a model, have it interpret the findings, and generate a remediation path for whichever repository you name. A year of work, a service to babysit, and a user nobody had met yet.

I wrote two skills and a task instead.

## The build

An agent gathered every CVE it could reach. I passed the ones it could not to a second agent and let that one finish the list.

With the research in hand, I asked for a runbook that remediates the findings. Then I asked it to split the runbook by ecosystem. Then I asked for a version written for a developer who wants to copy, paste, and validate — the audience was part of the request, not a note at the end. That version became a skill.

The second agent brought up VMs and ran the runbooks against them. Testing at my job happens on those VMs; the agent was doing the verification we already do, at a speed we do not. It came back with comments. I asked for a V2, we bounced it once more, and moved to the next application.

The last piece was a skill that runs the flow in podman, so the next person can use it without me.

Two skills and a task. No service, no roadmap, nobody waiting for a release.

## What changed from the way I used to work

My first year with these tools was the wrong decade with a faster engine. I kept the habits: pick the project, plan it, write it, ship it — with a model doing the typing. Projects built that way are hard and expensive to keep. They also run into the question that ends most internal tools: who are the users? A platform needs an audience for years. A skill needs one afternoon and one person who wants the finding closed.

The gap between people who use this and people who do not is wide right now, and it closes from the bottom. The assistant that today needs a good prompt will, in a couple of years, need a sentence. The phone did that to computing: at some point it stopped being a device you had to learn and became the thing in everyone's hand. The assistant is on that road, and most of the people who will use it are not reading this blog.

One model carries almost all of my work, Sonnet, and the credits for the week last. That is the boring part of the story, and it is the part that decides whether any of this fits in a real job.

## Second order

The skills I wrote this month will be obsolete before the year ends. I expect that. When one stops working I write the next one, and the next one takes less time because the failure is familiar.

What carries over is not the artifact. It is the judgment about what deserves a skill, what gets tested before anyone trusts it, and who the guide is written for. Those three decisions are the work now.

I still fix my own vulnerabilities. `gh`, a clone, TDD, and the issue URL or the scanner export in the prompt. Reading the diff is the part I kept, and the part I will keep arguing about in reviews.

I think like an agent now, which is a strange sentence to write. When a problem lands I inventory the surfaces: the API, the document, the cookies that let a browser session do what the interface will not, a diagram, a skill. Most of it gets used once. When the goal is reached I drop the script and start the next task from zero. There is no codebase to maintain because there is no codebase.

## The trade

I will not master my field again. The depth I spent years building is not the depth this work asks for, and that took a while to say without flinching.

What I have instead is fluency with my computer and my time. A task that used to sit in a backlog for a quarter now takes an afternoon and a bounce. It feels like the year I learned Rails, down to dreaming in Ruby and ActiveRecord. A tool changes what a day looks like, and then it changes what you think about while you fall asleep.

The imagination is the limit, which sounds like a slogan until you notice how much of the work is deciding what to ask for. That decision has not been automated. It is the job, and it is the part I am still learning.
