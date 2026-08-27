---
title: "Skills Suck. Scripts Don't."
date: 2026-08-26 20:50:00 -0600
categories: [AI, Workflow]
tags: [ai-lab, codex, agents, skills, automation, llm, makefile, devops, project-writeup, weekend-project]
description: "How I tried moving my AI automation from scripts to skills, watched it hallucinate on unstructured work, and moved back to a Makefile — and why engineering thinking is still the moat."
image: /assets/img/posts/2026-08-26-skills-suck-scripts-dont.png
---

> **TL;DR** — I moved my AI automation from stable scripts to agent "skills" for unstructured tasks (dashboards, Monday updates, Airtable reads, reports). It hallucinated exactly where the work was unstructured — stale data, renamed columns, missing meeting context. The fix was old-school: a Makefile, audited sources, and me running the commands.
>
> - Skills are great for guided, repeated work. They are a bad fit for human-like judgment tasks.
> - Hallucinations concentrate where context is loose: columns, comments, meetings.
> - Deterministic beats probabilistic when the task is stable enough to encode.
> - Engineer thinking — problem decomposition, auditing, structure — is still the skill that matters.

I have been happily automating my job with AI for a while. Scripts, mostly, written with Codex. Not perfect, but stable.

Then I got fancy. I moved the repetitive stuff into agent skills — no code, just a skill file telling the agent what to do. Generate dashboards. Update Monday. Pull Airtable. Write reports. At first it felt like the future.

Then the hallucinations showed up. And they showed up exactly where the work was unstructured.

## The turn to skills

The seduction was real. The tasks looked like prompts, not programs. A dashboard here, a board update there, a report pulled from a table. Each one felt too small and too variable to justify a script — columns change, comments appear, priorities shift. A skill that could *reason* about the mess seemed like the obvious tool.

And it worked. At first.

The agent filled in gaps, handled the variation, produced reasonable output. It even made the mess look manageable. That was the trap.

## Where it broke

Hallucinations arrived quietly. A report citing a column that had been renamed last week. A dashboard showing a status that contradicted the source. A summary missing the context from a meeting I was sure it had seen. Nothing catastrophic, individually. Collectively: trust gone.

The pattern was consistent. **The more unstructured the task, the more the agent invented.** Stale information got treated as current. Contradictory data got reconciled by *guessing* instead of flagging. Missing context from conversations got filled in with plausible-sounding fiction.

This is not a bug report against one vendor. The [arXiv survey on LLM-agent hallucinations](https://arxiv.org/abs/2509.18970) catalogues it as a structural property: agents hallucinate on factuality and on faithfulness — inventing facts, and drifting from the instructions. And it propagates: [AgentHallu](https://arxiv.org/abs/2601.06818) shows intermediate-step hallucinations *travel along the trajectory*, so one wrong assumption early corrupts everything downstream. One renamed column, one stale read, and the whole report quietly lies.

## What I learned

The lesson was not "AI is bad." The lesson was about **where the structure lives**.

[Anthropic's own guidance](https://www.anthropic.com/engineering/building-effective-agents) draws the line I had blurred: *workflows* orchestrate LLMs through predefined code paths; *agents* let the LLM direct its own process. Their explicit advice — workflows beat agents whenever the task's structure is stable enough to encode in code. My dashboard-and-report tasks were stable enough. I had handed them to the less deterministic tool on purpose, because it looked modern.

| Layer | What it is | When it wins |
|---|---|---|
| Workflow / script | Predefined code path, auditable | Task structure is stable enough to encode |
| Agent skill | LLM directs its own process | Task genuinely needs reasoning + judgment |
| Human | Judgment, accountability | Stakes are high, failure is expensive |

The industry has been circling the same conclusion from the other direction. [Kong's deterministic AI architecture piece](https://konghq.com/blog/engineering/deterministic-ai-architecture-enterprise-reliability) argues the reliability win comes from making the *control plane* deterministic — auditable scripts and API calls — so the probabilistic model only does the parts that need judgment. [Elementum's human-in-the-loop writeup](https://www.elementum.ai/blog/human-in-the-loop-agentic-ai) says it plainly: deterministic rules where consistency is required, agentic reasoning where it adds value, human judgment where stakes demand accountability.

I had inverted the stack. I made the whole thing probabilistic and removed the deterministic layer. The fix was to put it back.

## The fix: a Makefile

The unglamorous truth: I wrote a Makefile. The same commands the agent used to run, now run by me.

- `make dashboard` builds the dashboard from the source data.
- `make monday` pushes the board updates.
- `make report` generates the report from the audited sources.

The scripts are deterministic. The inputs are defined. The sources are structured *before* anything touches them — that was the audit step: I looked at what the script was doing, cleaned up the column definitions, pinned the data sources. Now the machine does the machine parts and I run the machine.

It saves me time, headaches, and tokens. Three wins, none of them fancy.

## When I still use a model

When a command gets too complicated, I do not fight it. I hand the *execution* to a small model — Haiku usually — with the variables and files spelled out, and let it run the thing. This is the same [smart-model-plans, cheap-model-builds split](https://blog.oliveira.ninja/posts/smart-plan-cheap-build/) I wrote about before: the plan lives in the structure, the small model just executes. It works most of the time, and when it does not, the failure is cheap and visible.

The difference from before: the model is now the *worker*, not the *architect*. It has bounded inputs, defined outputs, and a human who checks the result. That is the human-in-the-loop shape that [IBM's governance analysis](https://www.ibm.com/think/insights/liability-laundering-problem-human-in-the-loop-not-governance-strategy) insists on: not a suspicious human watching everything, but a system whose steps are transparent enough that the human has real evidence to interrogate.

> "The model is the worker, not the architect. The plan lives in the structure."

## The moral

Here is the part I keep coming back to. Problem solving like an engineer — decomposing the task, structuring the sources, auditing what the automation does, deciding where the judgment goes — is *still* the valuable skill. Maybe more valuable now.

The tools changed. The discipline did not. In a world where people think less of those who used to code for a living, the people who can still think in systems are the ones the automation does not replace — they are the ones who decide what the automation is allowed to do.

That is a skill no agent has yet.

*Update: for the record, this post was written the same way — drafted by an agent, structured and audited by a human, shipped through a pipeline that is itself the argument.*
