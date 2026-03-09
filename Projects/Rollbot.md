---
title: RollBot - A SaaS Discord bot for Tabletop Roleplaying
slug: rollbot
status: ongoing
startDate: 2026-03-01
endDate:
tags: [career, saas, devops, tabletop_roleplay]
repo: https://github.com/dorlo1994/rollbot
demo: false
featured: true
relatedPosts: 
---
## Background
### Dungeons and Dragons
Back in 2020, I was running a Dungeons and Dragons campaign with a group of friends. For anyone unfamiliar with the game, Dungeons and Dragons (D&D for short) is a tabletop fantasy roleplaying game, usually for 5-7 players, centered around the players' characters going on quests in some fantastical setting. One player is the Game Master (GM), who describes the scenes playing out, while each other player plays their own character interacting with the world.

Most interactions in the game are handled by the **D20 Test**: a player attempting a challenge rolls a 20 sided die, adds some integer modifier to the result based on the nature of the challenge (skill modifier), and the result is compared to a target number set by the GM to reflect the difficulty of the task. If the player's number is equal to or higher than the target - their character succeeds the challenge, and fails otherwise. Each character's different skill modifiers are detailed in their **Character Sheets**, along with many other details of the character.
### The Problem
Like many other groups we moved our games to an online space; the Discord platform. However, a minor table problem now became a major issue: players who aren't familiar with the game's rules and rely on others at the table to help them out, are now alone with their character sheets, unable to parse them, and therefore can't participate in the game.

As a recent Computer Science graduate, I took it upon myself to provide a digital solution to this issue. I designed a Discord bot user which holds each player's character sheet, and can perform D20 tests, as well as other dice rolls required by the game. It was barebones, ran locally on my laptop, and eventually ran into issues when a different bot was introduced into the server. It worked for its' narrow purpose, and never ran again after that campaign came to an end.
## The DevOps Cycle
As of recently, I am expanding my portfolio of software design projects. A major goal of this expansion is to familiarize myself with modern software design paradigms, such as those outlined in the (poorly named) 12 Factor App. Moreover, I am aiming to gain practical experience with real Cloud infrastructure with a real application, as opposed to a simple "Hello World" project.

To that end, this project aims to implement a complete **DevOps Cycle** for an actual live service, and utilize this implementation to maintain and further develop this service. Let us expand on the components of that cycle:
### Plan
This refers to abstract problem solving, assessing the needs of the users, the scope of the application etc. In my example, this came down to mapping out the character sheet data structure and the actual commands to implement.
### Code
Implementing plans as code, and managing the resulting codebase. In my "hacky" solution, I just kept the javascript files locally on my PC, not even as a git repository (sidenote: it is MADNESS that git is not taught in a CS Bachelor's degree). With git and GitHub, I not only gain codebase management, but also gain access to tools which will be useful on following steps.
### Build
Compilation, packaging, and any other process that results in artifacts that can deploy our program on a host. Previously, since my own machine was also the deployment target, and javascript is not a compiler language, this step was invisible. With modern cloud servers as a deployment target, however, building becomes entirely different.

The main difference is that the development environment is no longer identical to the release environment. My development environment contains development tools, secrets such as service tokens, and specific computing resources and specs. These all need to be handled differently on different environments, which can be easily ensured via Docker Compose, building a docker image (or a set of such images) which mange environment differences.
### Test
As the complexity of the software rises, the likelier it is that bugs and errors arise. Ideally, we'd like to detect and eliminate any of them *before* users start using the product. To that end, we can implement automatic tests which detect a wide range of unexpected behaviors in the codebase.

GitHub provides a tool for such tests and more: GitHub Actions. It allows us to define workflows to be executed on certain triggers (such as pushes to main), which in this case is our test suites. Since the new bot is implemented in Python and not javascript, these tests will be implemented with PyTest.
### Release
Once a build has passed every test, it can be published and viewed by the user base. These releases are versioned, to maintain a clear history of the software and allow access to previous releases, some of which may not be supported.
### Deploy
Most modern applications are not run from the users' host directly, but are services with small scale client applications (mostly browsers) making requests to those services. Deployment refers to the process of setting up these servers with our application. This project is an example of such an application, as just one bot can serve any number of users in theory.

In practice, the deployed application must be designed to handle the load of requests sent its' way. How this is done is its' own entire topic, one which I will expand on when I've handled enough real world data to have any insight on.
### Operate
An application being released and deployed is never a smooth process. User transition may be rough, new bugs may be revealed, and conflict between already running components of the applications and the new deployment may arise. These are all part of the operations phase: maintaining a live service with actual clients, gathering input from them, etc.
### Monitor
Data should not only be collected from the user base, but from the application and its' infrastructure. Responses to requests can be timed, computing resource usage can be measured, and any error can be automatically sent to the maintainer. Tools like Grafana already offer a variety of implementation to these ends.

The collected data leads to further planning, coding etc., thus the term DevOps Cycle.
## Goals
1. Implement every step of the DevOps Cycle.
2. Further develop the capabilities of this bot:
	1. Attack and Damage rolls.
	2. Other tabletop systems.
	3. Out of session utilities: scheduling, reminders, etc.
3. Possible: Implement a web UI for character / campaign management.