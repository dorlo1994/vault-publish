---
title: "Designing My Personal Website as Infrastructure"
date: 2026-02-11
type: blog
tags: [architecture, devops, ci-cd]
draft: false
---

## Motivation

I wanted a personal website that serves both as a business card and as a software project, with real infrastructure design, which I maintain. In setting up this project, I keep the following constraints in mind:
## Constraints

- No backend
- No database
- Minimal cost
- Fully static deployment

This gives me clearly defined boundaries, allowing me to clearly focus on the ingestion pipeline itself. Moreover, this prevents scope bloating.

## Architecture
The publishing pipeline follows this structure:
Obsidian → Content build script → Vite → GitHub Actions → Static hosting

### Obsidian
Treated as a singular source of truth, Obsidian is an open source markdown editor, which I am using to write and edit my files. It is also used as a PKM, but that is currently irrelevant to the website.
Obsidian's workspace is known as a Vault; it encapsulates files, metadata, settings, and plugins (including community made plugins). I introduced an internal vault, `vault-publuish`, which is known as the publishable vault. This is where I write `.md` files that are eventually rendered in my website (like this one!)
### Content Build Script
On the website side, the NPM build script also scrapes the publishable Obsidian vault for relevant markdowns, normalizes them and defines their relevant metadata into a `.json` file.
The crux of the metadata is the **slug**: the name of the page as it will end up in the URL. For this page. the slug is `blog/personal-website-project`, as ingested from the filename `Blog/Personal Website Project.md`.
### Vite
The way I think of Vite is as a "compiler" for react.js. Once I see the new pages being properly compiled in the website, I move onto the next step.
### GitHub Actions
This is where CI/CD lives, and currently has a build test which tests for build artifacts. The next step in the process is getting the server to a static host and revealing the IP address.
### Static Hosting
Currently, the website is personally hosted on my laptop. A further goal of this project is setting up a load balancer and a cloud mirror, which will serve as a backup in case my laptop isn't online. This choice will reduce hosting costs, as I only host on cloud when necessary.
The domain name and DNS are provided via Cloudflared.
## Conclusions
What I have now is a minimalistic software project which I can maintain and further develop, as well as document my process. Even now a build test on GitHub Actions was enough to expose several architectural failures as well as exposing me to new development concepts in modern react.
## Follow ups
As of right now, this pipeline is the bare minimum needed to get the website up with some presentable content. However, there are many potential improvements:
### Tasks
1. **Fully automate CD**: as of right now, I need to manually push and pull into `vault-publish`. I should set up services which sync these automatically.
2. **Externalize Metrics**: I would eventually want to present this website's DevOps metrics. Imagine reading a blog post from some date which details how I reduce build times, which you can then directly observe on this website.
3. **Subscription and Mailings**: If and when blog posts become regular, I'd like to have an email sent out to anyone who subscribes to this blog. Right now this is entirely out of scope, but this will let me have some security experience.

Each one of these tasks tackles its' own research questions, which are my main motivators when it comes to any personal project. I hope to have more to share with you all as soon as possible.