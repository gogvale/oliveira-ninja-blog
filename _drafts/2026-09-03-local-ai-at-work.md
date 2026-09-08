---
title: "The Reasonable No That Creates Shadow AI"
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, opinion, security]
description: "Security said no to a client MCP. We almost built our own AI for the demo. That is how shadow AI starts."
draft: true
---

<!--
PROVENANCE (strip at publish):
- Lived anchor (2026-09-08): Gabriel's team wanted to use a client vendor's MCP to serve the client better. Rejected for lack of an MCP policy — a fair call. The team briefly considered running their own AI for the demo; they did not, because it is not the right thing. The shadow-IT reflex is the story.
- Older seed (2026-09-03): anonymous forum thread (no. 109677087) "boss said no local AI -> self-funded Claude sub" — kept only as the idea-level local-AI pattern.
- Never cite the thread. Re-ground any borrowed claim in a legit source before publishing.
- Never name the employer or the client. "A client's vendor" stays generic.
-->

> **TL;DR**
> - Security said no to a client's MCP server — no policy for it yet. The call was fair.
> - The no came with no alternative path, and the team briefly considered running their own AI for the demo.
> - That reflex is the birth of shadow AI: rules block intentions, and people take the least-friction path.
> - The problem with shadow AI is not the tool. It is that governance never sees the data.

A few months ago, our team wanted to use an MCP server from one of our clients' vendors. The client would get better service, faster — the tool was built for exactly that. Security said no. The company had no policy for MCPs yet: no review path, no approved list, no precedent.

They were right. An unreviewed third-party tool gets access to whatever context you feed it. Saying yes without a policy is how client data leaves the building through a chat window. The call was correct, and I would defend it.

The part I keep thinking about is what happened next. We wanted the tool. The approved path did not exist, and nobody offered an alternative. For a moment — one honest moment — the shortest path looked like running our own AI for the demo. No approval. No policy. No governance. Just a model and a task.

We did not do it. It is not the right thing, and we knew it. But that moment is the story. It is the exact moment shadow AI is born: a reasonable no, a blocked intention, and a team looking for the least-friction path instead of the governed one.

## The no was right

This is not a post about a bad security team. This is the part that makes the whole thing uncomfortable: the security team was correct.

An MCP server is a standing channel into your context. The vendor's tooling, the client's data, your prompts, your code — everything flows through it. Approving one without a policy means approving it without review, and review exists because vendors differ. Some are careful. Some are not. You can not tell from the marketing page.

The absence of an MCP policy is not a reason to reject MCPs forever. It is a reason to reject them until the review exists. That distinction matters, because the employees on the receiving end of the no rarely hear it.

## A no without a path is a friction machine

The pattern is older than AI. Security blocks something to protect the company, and the block lands on an employee with a job to do. The employee does not fight the policy — policies are hard to fight. They find the shortest path around it.

Shadow IT was born this way: rules created for security, blocking the intentions they never meant to block. Every IT team knows the list — the spreadsheet that lives on a personal drive, the VPN nobody approved, the tool paid for with a personal card.

Shadow AI is shadow IT with a data problem. The ungoverned tool is not a spreadsheet. It is a model that reads the conversation.

> The problem with shadow AI is not the tool. It is that governance never sees the data.

## The counterpoints that hold up

The reasonable no has real weight behind it. Three arguments survive the scrutiny.

**The policy gap is real.** You can not review what you have no process for. "Until we have an MCP policy, we do not approve MCPs" is a legitimate position — the alternative is approving tools one by one with no standard to hold them against.

**The downside is not symmetric.** A rejected MCP costs us a better demo. A leaked client conversation costs the client relationship, and the legal exposure that follows. Security is paid to weigh the second outcome, not the first. When the downside of being wrong is that large, the conservative call is not cowardice — it is arithmetic.

**Rolling your own is not automatically safe.** The shadow path we considered — our own AI for the demo — would still ship with telemetry, still need patching, still have no audit trail unless someone built it. The governance problem does not disappear because the tool is yours. It moves from the vendor to you, and nobody had signed up to own it.

## What the "no" actually means

The team heard a rejection. What was actually said was a status report.

| What the team hears | What is actually said |
|---|---|
| "There is no MCP policy" | "We have no way to review it safely yet" |
| "Do not roll your own AI" | "Ungoverned models scare us more than the vendor" |
| "Why do you need this?" | "Nobody here wants to own the risk" |

Every "no" needs a door. The teams that build shadow AI are not the ones with rebellious employees — they are the ones where the shortest path to a useful tool is the ungoverned one.

We did not build the shadow demo. Most teams do not. Most people want to do the right thing, and the margin in our case was a conversation away from going the other way. The fix for shadow AI is not a louder no. It is a path: a review process with an answer attached, a sandbox where a team can prove a tool against real data without real risk, a policy that says yes-with-controls instead of no-for-now.

Governance that only blocks teaches people to route around it. Governance that provides a route does not need to block as often.
