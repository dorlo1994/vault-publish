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

This gives me clearly defined boundaries, allowing me to clearly focus on the ingestion pipeline itself.

## Architecture

Obsidian → Content build script → Vite → GitHub Actions → Static hosting