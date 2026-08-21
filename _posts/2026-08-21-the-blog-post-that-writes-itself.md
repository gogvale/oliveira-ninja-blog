---
title: "The Blog Post That Writes Itself"
date: 2026-08-21 13:30:00 -0600
categories: [AI, Writing]
tags: [ai-lab, writing, blog, attention, llm, workflow, hermes, project-writeup, weekend-project]
description: "How I built a blog-writing pipeline with an AI agent — the rules, the research, and the part only a human can do."
image: /assets/img/posts/2026-08-21-the-blog-post-that-writes-itself.png
---

> **TL;DR** — This blog now runs on a writing pipeline: an AI agent drafts, I shape. It works because we agreed on rules first.
>
> - Good posts are structure plus voice: hook, TL;DR, scannable sections, one idea per paragraph, real visuals.
> - Attention spans are short: millennials ~11s, Gen Z ~6-8s. Readers scan; they do not read.
> - The agent does the research and the typing. The human in the loop decides what sounds like a person.
> - The rules live in a skill file, so every post gets the same treatment and the blog stays maintainable.

Somewhere in the last week, this blog stopped being something I wrote and became something I direct. That sounds lazy. It is the opposite.

I spent a session with my AI agent building the pipeline that now produces these posts. We studied attention spans, read the research on scannable writing, argued about em dashes, and turned it all into a rulebook the agent follows every time. This post is about what good blog posts are made of, and about the part that still needs a human.

## What good posts are made of

The rules turned out to be simple. Almost boringly simple.

**Structure beats inspiration.** Every post now opens with a TL;DR so a brain-fried reader knows in three bullets whether to keep going. Then a hook. Then sections a scanner can follow by headline alone. The agent's rulebook says the subheads alone must tell the whole story, because for most readers, they do.

**Attention spans are real.** The research is stark: millennials hold focus for around 11 seconds, Gen Z for 6 to 8, Gen Alpha even less. Nobody reads a wall of text. So the rules enforce short paragraphs, one idea each, and a visual or structural break every 250-350 words — a diagram, a table, a pull-quote, a list. Something to catch the eye as it scans down the left edge.

**Voice is a checklist, not a mood.** My agent and I wrote down what my writing actually sounds like: minimal contractions, dry humor, concrete details, em dashes that earn their place. Then we built a second rulebook, the anti-slop list, that catches the phrases AI reaches for on autopilot: "Here's the thing", "Let that sink in", and every adverb in the dictionary. The first draft of this very post had three of them. The gate caught them before publish.

## The target group

Every post starts with a question: who is reading this, and what will they scan for? This blog targets security and automation engineers — mostly millennials and older Gen Z. That shapes everything: they want concrete stories over generic advice, they skim for commands and numbers, and they will judge the post in the first two sentences. The TL;DR exists for exactly that reader.

## Where the agent helps

The agent does the heavy lifting in three places:

1. **Research.** It searches, reads, and pulls the actual sources. For the post about cheap models, it found Anthropic's official agent-patterns doc, a 400-PR benchmark, and the Reddit thread that confirmed the experience. The good sources get linked; the blog-level ones get woven into the argument.
2. **Implementation.** It writes the draft, generates the banner images, validates the front matter, commits, pushes, and watches the deploy. The whole publish loop is scripted.
3. **Consistency.** The rules live in a skill file, not in the agent's memory. Every post gets the same structure, the same gates, the same checks. That is the maintainability win: the blog does not drift post to post.

## Where the human wins

The agent is a great first draft. It is a terrible final judge.

The ten-fold difference comes from the human in the loop. I decide what sounds like me. When the anti-slop gate flags a sentence as "AI writing" but it is actually my voice — the negative-listing joke, the "pretty good weekend" closer — I keep it. When the agent proposes a structure that reads like a whitepaper, I cut it. When it finds a perfect diagram from the source, I check it actually shows what we claim before it ships.

The agent optimizes for rules. I optimize for the reader I know. That combination is the whole trick: rules make the agent consistent, and the human makes the result sound like a person.

> "The agent optimizes for rules. The human optimizes for the reader."

## The pipeline

| Step | Who | What |
|---|---|---|
| Idea | Human | The story, the angle, the target reader |
| Research | Agent | Sources, benchmarks, community confirmation |
| Draft | Agent | Structure, TL;DR, sections, gates applied |
| Edit | Human | Voice, cuts, keeping what sounds like me |
| Banner | Agent | Generated image matching the post |
| Publish | Agent | Front matter, commit, deploy, verify |
| Share | Human | Approve the LinkedIn post, write the hook |

That division has produced six posts in two days, each with a banner, a TL;DR, and a deploy verified end to end. The fastest part is the typing. The slowest part is the thinking, and that part is mine.

## What I learned

- Writing rules down turns taste into process. The blog is now consistent because the rules do not live in anyone's head.
- Attention is the scarcest resource. Every sentence competes with a notification.
- AI multiplies effort when the human sets the direction. It compounds mistakes when nobody is looking.
- The best tool is the one that makes you the editor, not the typist.

The fun part is that this post is the proof. It went through the same pipeline it describes — drafted by the agent, shaped by me, gated, banned, verified, deployed. The banner is agent-made. The judgment is mine.
