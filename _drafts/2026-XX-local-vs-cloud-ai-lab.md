---
title: "Lab idea: local AI vs cloud AI — same project, both paths"
date: 2030-01-01 00:00:00 -0600
categories: [AI]
tags: [ai-lab, lab-seed]
draft: true
---

<!--
SEED (Gabriel, 2026-09-08) — parked. Future lab, do NOT announce in the
original post (2026-09-03-local-ai-at-work.md). When published, it is the
"Part 2" that tests and corrects the original's claims.
-->

## The lab
Build the SAME real project twice, back to back:
1. **Local path**: local models (llama.cpp/GGUF class), running on the developer's own machine — no server, no cloud.
2. **Cloud path**: a cloud assistant (Claude/Copilot/Codex class).

## What to measure (the original post's claims, put on the stand)
- **Results**: output quality on the same tasks — where is "good enough" actually good enough, and where does local fail?
- **Difficulty to set up**: install-to-first-useful time for each path.
- **Costs**: real numbers — hardware (does it need new gear?), electricity, per-seat subs.
- **Minimum specs**: what is the floor for a usable local setup (RAM/VRAM, CPU-only OK?), and how far below a "GPU box" it really is.
- **How to secure the local path**: egress control, constrained tool access, no secrets in context, sandboxing — the checklist the original post claims a permissive policy needs.

## Optional (if Gabriel gets creative)
**Adversary simulation**: a prompt-injection payload embedded in fetched content, aimed at the local agentic setup — does it exfiltrate through the TOOLS (shell/files) even with the network locked? Tests the original post's sharpest edge: "the model can not leave — but the terminal it invokes can."

## Output
Part 2 blog post: what the lab confirmed, what it corrected from the original (2026-09-03 local-ai-at-work draft — "The Door Is a Local Model").
