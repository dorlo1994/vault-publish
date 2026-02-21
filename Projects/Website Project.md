---
title: Personal Website Project
slug: website-project
status: ongoing
startDate: 2026-01-15
endDate:
tags: [reactjs, web_design, career, fullstack]
repo: https://github.com/dorlo1994/personal_website
demo: false
featured: true
relatedPosts:
---
## Goals
Starting out on this project, I had two goals in mind:
1. Create a digital business card and portfolio page
2. Have this website be one project displayed in my portfolio

However, as this project took shape I decided to frame it not only as a personal website, but as a frontend to my personal platform. This ties the website to my personal automation platform project, and to my greater automation and research goals.

Another goal I have in mind is to expand my personal DevOps knowledge, by productizing the website as cleanly as I can.

## Stack
The website is designed in React.js and rendered by Vite, with some basic plugins. The site itself is static once built, with blog and project posts generated from Markdown files, originally created in Obsidian and rendered in HTML via the markdown-it plugin.

CI is currently implemented in GitHub Actions, with a smoke test building the website from scratch and ensuring all pages exist.

As of this moment, the website is hosted on GitHub Pages, and accesses project demos via exposed API endpoints, with the demos themselves hosted elsewhere. This allows me to get practical experience with DNS services, reverse proxying, and much more.

## Future Goals
1. Create a search function in the blog and projects pages.
2. Expose internal metrics such as uptime, build times, CI status, etc.
3. Continue integration with personal automation.