---
title: "The Door Is a Local Model"
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, opinion, security]
description: "Security said no to a client MCP. Every no needs a door — and the door nobody talks about is the offline model: zero egress by construction, no IP story, good enough."
draft: true
---

<!--
PROVENANCE (strip at publish):
- Lived anchor (2026-09-08): Gabriel's team wanted to use a client vendor's MCP to serve the client better. Rejected for lack of an MCP policy — a fair call. The team briefly considered running their own AI for the demo; they did not, because it is not the right thing.
- Pivot v3 (2026-09-08): the constructive answer to "every no needs a door" — the door is the offline/local model. Permissive policy for zero-egress PoCs with a security checklist.
- Older seed (2026-09-03): anonymous forum thread (no. 109677087) — idea-level local-AI pattern only. Never cite.
- Do NOT mention any future lab/comparison post in this piece.
- Never name the employer or the client. "A client's vendor" stays generic.
- Research digest: ~/shadow-ai-research-digest.md (2026-09-08) — background only.
-->

> **TL;DR**
> - Security said no to a client's MCP server — no policy for it yet. The call was fair; an unreviewed channel into our context is a real risk.
> - The no had no door, and for a moment the team considered running their own AI for the demo. That reflex is how shadow AI is born.
> - Every essay about shadow AI ends at "give employees secure alternatives." Nobody says what the alternative is.
> - It is the model that can not leave: offline, zero egress, no IP story, good enough. A permissive policy for local-model PoCs is the door.

A few months ago, our team wanted to use an MCP server from one of our clients' vendors. The client would get better service, faster — the tool was built for exactly that. Security said no. The company had no policy for MCPs yet: no review path, no approved list, no precedent.

They were right. An unreviewed third-party tool gets access to whatever context you feed it. Saying yes without a policy is how client data leaves the building through a chat window. The call was correct, and I would defend it.

The part I keep thinking about is what happened next. We wanted the tool. The approved path did not exist, and nobody offered an alternative. For one honest moment, the shortest path looked like running our own AI for the demo. No approval. No policy. No governance. Just a model and a task.

We did not do it. It is not the right thing, and we knew it. But that moment is the story — the exact moment shadow AI is born: a reasonable no, a blocked intention, and a team reaching for the least-friction path.

The same reflex plays out smaller every day. A developer asks to run a local model. The answer is no. The developer self-funds a subscription, because it is cheap and the answer is now. Only the budget changed.

## The no was right

This is not a post about a bad security team. The security team was correct, and the whole argument only works if we keep that on the table.

An MCP server is a standing channel into your context — the vendor's tooling, the client's data, your prompts, your code. Approving one without a policy means approving it without review, and review exists because vendors differ. Some are careful. Some are not. You can not tell from the marketing page.

The absence of an MCP policy is not a reason to reject MCPs forever — only until the review exists. The problem is that the employees on the receiving end of the no never hear that distinction.

## A no without a path is a friction machine

The pattern is older than AI. Security blocks something to protect the company, and the block lands on an employee with a job to do. The employee does not fight the policy — policies are hard to fight. They find the shortest path around it.

Shadow IT was born this way: rules created for security, blocking the intentions they never meant to block. Shadow AI is shadow IT with a data problem. The ungoverned tool is not a spreadsheet. It is a model that reads the conversation.

**The problem is not the tool. It is that governance never sees the data.**

## Every "no" needs a door — nobody says what the door is

The shadow AI literature stops at the same place every time: do not ban, give employees secure tools they actually want to use. Fair. Now name the tool. Nobody does, because the honest answer is uncomfortable.

A cloud tool needs the slow review — data flows, vendor audit, contractual liability. And the market does not wait for it: capability climbs every release while the policy does not, so even an approved cloud tool arrives already one release behind.

The only category that clears review without a data-flow analysis is the one with no data flow.

## The door is the model that can not leave

The local-model threads keep proving the same thing: usable quality keeps landing on smaller hardware. A model that needed a rented GPU not long ago runs on a developer's laptop. No server. No per-seat cost. No vendor in the path. For our demo, it would have been enough — and the demo was the whole point.

Watch the governance math flip:

| | Cloud AI | Offline model |
|---|---|---|
| Where the data goes | leaves the building | stays on the machine |
| IP story | depends on the vendor | nothing to write |
| Audit | vendor logs | you own the silence |
| Cost | per seat, monthly | near zero, existing hardware |
| Review depth | deep, ongoing | a one-time checklist |
| Capability | frontier | good enough, closing fast |

A cloud tool is an approval. An offline model is a category you can approve once. The policy writes itself: local-model PoCs are permitted by default, with a security checklist — egress defined at the operating-system level, tool access constrained (no shell the model can invoke with your credentials), no secrets in context, and a data class it may see. Cloud tools keep the slow review, because they earn it. Offline tools get the fast lane, because physics earned it: the data can not leave unless someone makes it.

## The honest edges

The checklist exists because the model is not the boundary — the tools are. A local model with agentic tool access can still exfiltrate through the terminal it is allowed to invoke, prompt injection or not. Zero network does not matter if the model can ask the shell to do the reaching. That is why the policy constrains tools, not just ports.

The audit silence is real. A cloud vendor gives you logs as part of the price; an offline model gives you nothing unless you build the logging yourself. That is a fair trade for a PoC with non-sensitive data, and a bad trade for production workloads with client data. Scope the fast lane accordingly.

And "good enough" is task-dependent. For a demo, for a script, for a contained experiment — yes. For a large refactor against a ten-year codebase, sometimes not. A policy that names the tier instead of promising the world is a policy people trust.

## The no becomes a yes

We did not build the shadow demo. Nobody has to. The policy that says yes to the model that can not leave turns a reasonable no into a governed yes in a week — and keeps the data in the building.

> The safest AI tool is the one that can not leave the building.

Governance that only blocks teaches people to route around it. Governance that provides a route does not need to block as often — and the route can be a laptop with the network turned off.
