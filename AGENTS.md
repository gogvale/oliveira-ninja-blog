# oliveira.ninja Blog - Agent Guide

This repository powers `blog.oliveira.ninja`, a Jekyll/Chirpy blog for Gabriel
Oliveira.

## Purpose

The blog is for longer notes, experiments, and honest writeups around security,
automation, infrastructure, self-hosting, AI-assisted engineering, DevOps,
Linux, and pentesting.

The tone should feel like a tech-savvy friend explaining a weekend project:
playful and informative, with enough step-by-step detail to be useful, but not
overly dense or performatively technical.

## Content Tracks

Use these recurring tracks and tags when appropriate:

- `ai-lab`: vulnerabilities, auto-fixing, Hermes workflows, Langflow, agents,
  and experiments with AI-assisted security/engineering.
- `self-hosting`: homelab services, NAS, media, photos, backups, identity,
  sync, VPN, and maintenance.
- `degoogle`: replacing Google Photos, Drive, streaming, mail-adjacent tools,
  and other hosted services.
- `linux`: workstation/server tinkering, configuration, scripts, break/fix
  notes, and daily-driver experiments.
- `devops`: CI/CD, GitHub Actions, automation, infrastructure, and delivery
  workflows.
- `openshift`, `azure`, `kubernetes`: platform-specific DevOps notes.
- `pentesting`: only authorized labs or owned systems; include remediation and
  learning notes.
- `weekend-project`: small, fun, educational builds.
- `project-writeup`: writeups that can be spotlighted from the landing page.

## Writing Guidance

- Make the H1/title short and catchy.
- Prefer concrete stories over generic tutorials.
- Start with what triggered the experiment and why it seemed worth doing.
- Include a light step-by-step when it helps readers follow the path.
- Mention tradeoffs, dead ends, and decisions honestly — but only real ones.
- Use screenshots sparingly and only when they help. Avoid exposing secrets,
  tokens, private hostnames, or sensitive personal details.
- Avoid publishing security scans, pentests, or exploit details for systems the
  user does not own or have authorization to test.

### Keep posts short (Gabriel's explicit preference, 2026-08-20)

- Default shape: problem → discovery → setup → what changed (the win). Then stop.
- Do NOT add "What Got Weird", "What I Learned", or "What Comes Next" sections
  unless Gabriel asks for depth. Do NOT invent downsides.
- No closing flourish unless it earns its place. Target ~700–1100 words.

### Anti-slop rules (from hardikpandya/stop-slop, adapted)

- No throat-clearing openers ("Here's the thing", "It turns out").
- No emphasis crutches ("Let that sink in", "Full stop").
- No business jargon (navigate, unpack, lean into, game-changer, deep dive).
- No adverbs (-ly words; really, just, literally, genuinely, actually).
- No binary contrasts ("Not because X. Because Y."), negative listings,
  dramatic fragmentation, rhetorical setups ("What if...?").
- No false agency (inanimate things doing human verbs). Name the actor.
- Active voice. No vague declaratives ("The implications are significant").
- No Wh- sentence openers where a direct subject works better.
- Exception: em dashes are FINE and match Gabriel's voice — do not strip them.

## Post Template

Use `templates/post-template.md` as the starting point for new posts. Copy it
into `_posts/YYYY-MM-DD-slug.md`, update the front matter, and remove sections
that do not fit the story.

## Deployment

- The site deploys with GitHub Pages Actions.
- Do not add a committed `CNAME` file. The custom domain is configured through
  GitHub Pages settings/API.
- Local system Ruby may be too old; when local Jekyll fails, validate through
  the GitHub Actions build after pushing.
- After deployment, smoke-check the relevant public URL.
