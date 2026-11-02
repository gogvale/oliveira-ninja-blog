---
title: "Terra Costs Me More Than Sol"
date: 2026-11-01 09:30:00 -0600
categories: [AI]
tags: [ai-lab, codex, models, deepseek, cost]
image: /assets/img/posts/terra-costs-me-more-than-sol.png
description: "I moved the small edits at work from Sol to Terra and Luna. The invoice went down 50% and my afternoons got longer, because the savings come back as supervision."
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/terra-costs-me-more-than-sol.mp3">Your browser does not support the audio element.</audio>

> **TL;DR**
>
> - At work I run Codex on the 6.x models. Sol does what I ask and bills like the flagship it is.
> - I moved the small edits down to Terra and Luna: validations, formatting, a lookup across a few files. Terra lists at half of Sol's price per token.
> - The invoice went down. The checking did not. I spend the afternoon confirming what got done and correcting what did not, and that time is not on any price page.

At work I have been running Codex on the 6.x models for a while. Sol does what I ask. It also bills like the flagship it is, and most of what I ask on a Tuesday afternoon is not flagship work.

So I moved the small things down. Terra for most of it, Luna for the parts I could describe in one sentence. Validations, formatting, a lookup across a few files. I knew quality would drop a little. I expected a dumber version of the same worker.

## The first failure is not wrong code

It is a partial job reported as done.

I ask for four call sites, it changes two, and the reply says the file is updated. So I read the file. Then I ask again with the missing part quoted, and it changes the two remaining ones and breaks one that already worked. Then I ask for that one back, and it comes back with a different naming convention from the rest of the file.

Ask, confirm, correct, correct the correction. That is the loop, and it costs no tokens, because I am the one running it.

I am not reviewing code anymore. I am holding the hand of something that writes faster than me and will not read the file it is editing.

## What the price page does not show

Per million tokens, Terra lists at half of Sol and Luna at a twentieth ([prices](https://benchlm.ai/openai/api-pricing)). On paper that is the easiest decision in the world.

In practice I moved the cost from the invoice to my attention. Sol needs a glance at the diff. Terra needs me to write the ticket twice, check the file, and then explain what a validation is supposed to do. The cheaper model is cheaper only if I can walk away from it, and I cannot.

The other thing I did not expect: the small tasks are where I have the least patience to supervise. A domain lookup or a formatting pass is work I handed off *because* it is boring. If I have to sit through it now, I lost the reason I delegated it.

## DeepSeek Flash spoiled me

I run DeepSeek Flash on my own projects for a fraction of that. It picks the task up, reads the file, and comes back with the thing I asked for plus the part I forgot to mention. Maybe I am spoiled, and maybe comparing a personal agent to a work account is not fair.

But it broke the tier logic for me. Cheap was supposed to mean "do less and I will check the rest". Cheap turned out to mean "do less, and I will also have to notice that you did less".

## The test I want to run

What I have not done is let the cheap tier program, without me narrating the steps. That is the honest version of the experiment: not whether it can write code, but whether I can leave it alone for an hour.

I want to run it with the two crutches I already use on myself: caveman, which cuts my prose to the bones, and ponytail, which makes the agent walk down a ladder before it writes anything — does this need to exist, is it already in the codebase, does the standard library do it. Less code and fewer instructions is the failure mode I am trying to fix, so it may not help. It may make things worse in a way worth writing down.

When I get access to Anthropic, the same test goes there.

## Where the bill lands

I keep Sol for the work that has to be right and Terra for the work that has to be redone. Neither of those is a budget decision, and the cheaper model is not cheaper when I have to watch it.

A model that needs a babysitter does not cost less. It costs the same, and I am part of the bill.
