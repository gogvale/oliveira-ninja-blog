---
title: "You Are Paying for Prompts You Never Wrote"
date: 2026-09-11 15:15:00 -0600
categories: [ai-lab]
tags: [ai-lab, tokens, prompt-engineering, cost, hermes, deepseek]
description: "Every AI turn re-sends the whole conversation plus a system prompt you never wrote. That overhead is why the old prompt habits still save the most money."
image: /assets/img/posts/2026-09-11-token-cost-per-turn.png
---

<audio controls preload="metadata" style="width:100%;max-width:520px;display:block;margin:0 auto 1.2rem" src="/assets/audio/2026-09-11-token-cost-per-turn.mp3">Your browser does not support the audio element.</audio>

> **TL;DR**
>
> - Every question you ask an AI agent re-sends the entire conversation, plus a system prompt you never wrote and cannot see.
> - I cut that fixed overhead on my own assistant by 38% per call. It changed nothing about how I work.
> - Claude Code, Codex and the rest work the same way. A $200/month subscriber listed exactly what gets re-sent every single turn.

I asked my assistant to check its own bill.

The answer came back better than I expected — 56% less per API call than the week before. Then I read the breakdown, and the number that mattered was not the saving.

It was where the money goes. Most of every request is not my question. It is scaffolding I never wrote and pay for on every turn.

## The API does not remember anything

People complain that AI forgets.

It does not forget. It has no memory at all. An LLM API is stateless: every time you hit send, your client re-sends the whole conversation — every message, every reply, every tool result — as fresh input tokens. The model reads the entire thing again ([Oriveo](https://oriveoai.com/blog/ai-api-cost-comparison)).

So a long session does not cost a little more per question. It costs steadily more, forever, because you re-pay for the whole history on every turn.

Across one week of my own sessions, the average API call carried **253,000 tokens of context**. The question I typed was maybe forty of them.

Then there is the second layer. Before your conversation starts, an agent like Claude Code, Codex or mine ships a system prompt, a tool list, memory files and skill descriptions. All of it goes out again on every turn, relevant or not.

A Claude Max subscriber paying $200 a month filed a public issue about exactly this: [CLAUDE.md, rules files, memory index, skill listings and MCP tool schemas — all re-transmitted on every single turn](https://github.com/anthropics/claude-code/issues/46526). The `/context` dump in that report showed roughly **29,000 tokens of fixed overhead per turn**. System tools alone were 16,800 of it.

That is the rent. It is not on any invoice line you can point at.

## The 2023 playbook still pays

Models got better at reading intent. That part is real — you can be sloppy now and still get a useful answer.

The old prompt habits did not expire. Anthropic's current docs still prescribe the same list from 2023: be clear and direct, explain *why* the instruction matters, use examples (their word is [few-shot or multishot](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/claude-prompting-best-practices)), structure the prompt with explicit tags, give the model a role, put long documents at the top.

A smarter model compresses the gap between a lazy prompt and a good one. It does not close it. And one giant prompt covering everything is the expensive way to change nothing — you re-read it on every turn, while [long contexts degrade output quality](https://www.trychroma.com/research/context-rot) well before they run out of room.

Cheap beats clever. That has been true for three years.

## What I cut

My assistant runs on tokens I buy, so this is my money. The audit found a system prompt of 6,770 tokens per call and 115 skill descriptions loaded into it — the same [skill overhead I complained about in August](https://blog.oliveira.ninja/posts/skills-suck-scripts-dont/).

I turned 64 unused skills off. Dropped tools I do not call. Trimmed the memory file. Moved reasoning effort to low.

| | Before | After | Change |
|---|---|---|---|
| System prompt per call | 6,770 tok | 4,180 tok | −38% |
| Skill descriptions | 115 | 44 | −62% |
| Uncached input per call | 3,976 tok | 1,823 tok | −54% |
| Cost per API call | $0.00163 | $0.00072 | −56% |

Full disclosure: part of that 56% is not efficiency. My later sessions were shorter, so they carried less context. The guaranteed part is the system prompt, and it saves about **three hundredths of a cent on every call, forever** — because the prompt goes out again with every call, however small the question.

Three hundredths of a cent sounds like nothing. Multiply it by every turn of every session of every day, on a bill someone else writes without showing you the breakdown.

## The caching does not fix this

Prompt caching helps, and it is worth understanding before you feel good about it.

Cache reads are cheap. Anthropic charges [one tenth of the base input price](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) for a cache hit. OpenAI [discounts cached input by 50%](https://openai.com/index/api-prompt-caching/) automatically. My own lane bills cache reads at about two percent of the input rate.

Caching makes repetition cheaper. It does not make repetition disappear. The uncached part of the prompt still costs full price, reasoning tokens still bill at output rates, and every provider needs the whole prefix before it can match anything.

**You are not paying for your question. You are paying for the scaffolding around it.**

## What this means if you run these tools

Open the context panel in whatever agent you use. Claude Code has `/context`. Mine prints its own overhead. Find the block that is not your conversation, then cut it:

- Skills you never trigger.
- Tools you never call.
- Memory files you stopped reading in July.

It costs an afternoon and it pays out on every future turn.

> The model gets smarter every few months. The rent is due every turn.
