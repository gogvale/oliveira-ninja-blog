---
title: "Own the Skill. Rent the Tool."
date: 2026-09-03 06:40:00 -0600
categories: [Opinion, AI]
tags: [ai-lab, vibe-coding]
description: "FiletOfFish1066 automated his job for six years, got fired, forgot how to code. Vibe coding is that story in fast-forward — and the market still pays for the skill, not the tool."
image: /assets/img/posts/2026-09-03-own-the-skill-rent-the-tool.png
---

> **TL;DR**
> - In 2016 a Reddit user confessed to automating his own job for six years. He played games, collected $95,000 a year, got fired — and had forgotten how to code.
> - Vibe coding is that story in fast-forward, minus the paycheck.
> - The best-paying jobs still test what you can do by hand: Stripe and Jane Street ban AI in interviews. Code quality is the product again.
> - Lose the skill and AI stops being a tool. It becomes a toll booth between you and the work. You pay to work.

## He automated himself out of the skill

In 2016 a Reddit user going by FiletOfFish1066 confessed what he had done for six years: automated his own job as a fresh CS grad, played games during office hours, collected about $95,000 a year for it. [The post got around](https://www.reddit.com/r/antiwork/comments/ptmruv/programmer_fired_for_automating_job_playing_games/). He was fired. He had forgotten how to code — he had spent the six years on chemistry instead.

The story gets retold as a slacking cautionary tale. Read it again as a skill story: the automation was so good it replaced him, and he had nothing left to sell afterward. The script belonged to the company. The skill had evaporated.

## Vibe coding is the same arc, fast-forwarded

The FiletOfFish story took six years because he wrote the automation himself. Vibe coding collapses the timeline: you describe the work, the model does it, you review it like a manager who never learned the trade. You ship code you cannot explain, cannot debug under pressure, cannot defend in a code review.

The dangerous part is not that the code is wrong. The dangerous part is that it is often right enough — for months, for years — while the skill quietly evaporates underneath you. Then one day the tool cannot do your exact domain, or the subscription lapses mid-sprint, and the difference between you and FiletOfFish is that he still had his six years of paychecks.

The most public test of that arc ran in the open. ThePrimeagen — ex-Netflix, [a streamer whose reach in the developer world is hard to overstate](https://www.youtube.com/@ThePrimeagen) — spent three months heavy on vibe coding and [quit in February 2026](https://x.com/i/trending/2026795234840162434): the output felt subtly off, and it was robbing him of the craft. He went back to writing code by hand.

## The market still pays for the skill

The vibe coding discourse assumes every buyer wants shippable speed. A lot of them do. The ones paying the most do not.

The signal split is public now. **Stripe and Jane Street still ban AI in their interview loops.** Google's standard rounds do too, even while it pilots Gemini inside CoderPad; Meta added an AI-allowed round and measures candidates on it. Two philosophies, live in 2026 — [the firms paying senior money test what you can do with no tool in the browser](https://www.interviewcoder.co/blog/using-ai-for-coding-interviews).

The data side explains why. [GitClear's maintainability research](https://www.gitclear.com/the_ai_code_quality_maintainability_gap) tracks hundreds of millions of changed lines: code reuse is down 35%, duplicate blocks are up, short-term churn is up. AI-assisted codebases rot faster.

![GitClear maintainability chart: reuse signals eroding while duplication, copy/paste, and churn climb, indexed to 2023-2026](/assets/img/posts/2026-09-03-gitclear-maintainability-gap.png)

*Reuse signals (left) eroding as risk signals (right) climb with AI authorship — source: [GitClear](https://www.gitclear.com/the_ai_code_quality_maintainability_gap)*

The people paid to clean up rotting codebases are the ones who can still read code. [SonarSource's 2026 report](https://www.joberty.com/blog/why-companies-are-quietly-rehiring-software-engineers-in-2026-ai-reality-check/) puts 96% of developers as distrusting AI code without manual review. Hiring noticed: companies that over-hired vibe output are quietly rehiring engineers who can do the work.

| Vibe-coder assumption | What the market does |
|---|---|
| Speed is the product | Stripe and Jane Street ban AI in interviews |
| Shippable means good | GitClear: reuse down 35%, duplicates up, churn up |
| Review is a formality | SonarSource: 96% distrust AI code without review |
| Humans are the bottleneck | Quiet rehiring of engineers who can do it cold |

Those jobs are not dying. They are getting quieter and more selective — and they are exactly the ones that pay for expertise over output. Every year the AI-slop graveyard grows, those shops get more careful, and the premium on the human who can still do it goes up.

## When you pay to work

Here is the part nobody puts in the vibe coding pitch: the moment your skill atrophies, AI stops being free leverage and becomes a toll booth.

> "When your skill atrophies, AI stops being free leverage. It becomes a toll booth between you and the work."

Think like a freelancer, because that is where this shows up first. Every task now carries a metered cost. The tool owns the margin — the provider can raise the price, throttle the API, deprecate the model that knew your stack, and you cannot push back, because you no longer have the option of doing the work without it. You used to own the work. Now you rent it.

I wrote before about why [skills suck and scripts don't](https://blog.oliveira.ninja/posts/skills-suck-scripts-dont/). Scripts are deterministic: they run the same way every day, and when they fail you can read their code. A subscription is not that. A subscription is a lease on a mind — and the leaseholder sets the terms.

## Keep the skill warm

Use the tools. They are genuinely good, and refusing them is cargo cult in reverse. But keep doing the work by hand often enough that you could still do it if the lease ended tomorrow.

Write the script yourself sometimes. Read the generated code line by line. Take the occasional interview that tests you cold, even if you are not looking.

FiletOfFish1066 lost the job because his automation was better at it than he was. The rest of us are betting our rented assistant will not one day be better than us too — and the market is quietly betting the same thing, from the other side.
