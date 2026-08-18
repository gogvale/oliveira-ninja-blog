---
title: "Setting up oliveira.ninja, part 3: GitHub Pages"
date: 2026-08-18 01:30:00 -0600
categories: [Personal Site, GitHub Pages]
tags: [github-pages, dns, astro, jekyll, chirpy, weekend-project]
description: "How I connected oliveira.ninja and blog.oliveira.ninja to GitHub Pages without turning it into a giant infrastructure project."
---

Once the domain and email were working, I wanted the web side to exist too.

The goal was simple: `www.oliveira.ninja` for a small personal landing page, and `blog.oliveira.ninja` for longer notes like this one.

I did not want a complicated hosting setup. This was supposed to be a weekend project, not a new platform to maintain. GitHub Pages was the right fit: free, familiar, close to the code, and good enough for static sites.

## The shape of the setup

I ended up with two GitHub repositories:

- `gogvale/gogvale` for my GitHub profile README and the main landing page source.
- `gogvale/oliveira-ninja-blog` for the blog.

The first repo has two jobs:

- `main` keeps the GitHub profile README.
- `gh-pages` keeps the Astro source for `www.oliveira.ninja`.

The blog repo uses Jekyll with the Chirpy theme and deploys with GitHub Pages.

So the layout is:

- `www.oliveira.ninja` -> personal landing page.
- `blog.oliveira.ninja` -> blog.

## Building the landing page

For the landing page, I adapted the Gothsec Astro portfolio template. I liked the dark visual style and the animated LetterGlitch effect, but I did not want the page to feel like a generic portfolio template.

The page became a compact personal front door:

- A short hero section.
- A few selected projects.
- A small "Now / Lab" section.
- A writing section pointing to the blog.
- A concise about section.
- Links to GitHub, LinkedIn, and the blog.

I also kept the profile README separate. That was important because `gogvale/gogvale` is a special GitHub repository: its `README.md` shows up on my GitHub profile. I wanted the profile README to stay useful on GitHub, while the actual website lived in the Pages branch.

## Why I used GitHub Actions instead of branch-only Pages

GitHub Pages can deploy directly from a branch, but that only works cleanly when the branch already contains the final static files.

The Astro branch contains source code. It still needs to run a build step.

So the better setup was:

1. Push the Astro source to `gh-pages`.
2. Let GitHub Actions run the Astro build.
3. Deploy the generated static files to GitHub Pages.

That keeps the repository easy to edit and review. I do not have to commit generated build output.

## Connecting the domain

On the DNS side, I pointed the web records at GitHub Pages.

The useful records were:

- The apex domain, `oliveira.ninja`, pointing to GitHub Pages.
- `www.oliveira.ninja` as a CNAME to `gogvale.github.io`.
- `blog.oliveira.ninja` as a CNAME to `gogvale.github.io`.

I had originally considered committing a `CNAME` file, which is the classic GitHub Pages way. Instead, I configured the custom domains through GitHub Pages settings using the GitHub CLI and API.

That means the domain setting lives in GitHub Pages configuration, not as a file in the repo.

I like that split better for this setup because the repo branches have specific jobs:

- `main` is the GitHub profile.
- `gh-pages` is the landing page source.
- the blog repo is the blog source.
- GitHub Pages settings know which domain belongs to each site.

## Waiting for DNS and certificates

After that, the remaining step was mostly waiting.

DNS records need time to propagate. GitHub Pages also needs time to verify the custom domain and issue HTTPS certificates.

This was another place where `dig` helped. Instead of clicking refresh and hoping, I could check what DNS was actually returning:

```console
dig www.oliveira.ninja
dig blog.oliveira.ninja
```

Once the records resolved correctly, GitHub Pages could serve the sites. HTTPS followed after GitHub finished issuing certificates.

## The result

By the end, the setup looked like this:

- `www.oliveira.ninja` serves the Astro landing page.
- `blog.oliveira.ninja` serves this Chirpy blog.
- The GitHub profile README stays clean on `main`.
- Email works through Zoho.
- DNS is handled at Porkbun.

None of this is extremely advanced, but doing the full loop yourself is educational. You touch registrars, DNS, email authentication, static hosting, GitHub Actions, and HTTPS certificates in one small project.

That is a pretty good weekend.
