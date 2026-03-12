---
title: "RollBot Project Kickoff"
date: 2026-03-12
type: blog
tags: [devops, saas, tabletop_roleplay]
draft: false
---
## I. Introduction
Over the last few months, I have been steadily expanding my software development portfolio, with the explicit goal of gaining hands-on experience with modern cloud architecture, using real DevOps practices. While DevOps engineering work has been my professional bread and butter, most of that experience has been focused on on-premise infrastructure. A good project candidate, then, is any real service I can set up on a cloud host to handle real data. Enter: RollBot.

## II. The Original RollBot
Back in 2020, I was running a campaign of Dungeons and Dragons for a bunch of my friends. D&D is a tabletop fantasy roleplaying game, in which players' characters make decisions and attempt challenges, the outcomes of which are determined by combining a dice roll with some character data, found in each player's character sheet. 

When quarantine kicked in, our group moved to the Discord platform for online play. While this was a mostly smooth transition, it was rough for players who were not versed in the rules; at the table, they could rely on other players for help, but now they're alone with character sheets they can't parse.

To help with the game, I created RollBot: a simple javascript application running locally, holding character data for the players and capable of dice roll calculations. It was serviceable for this narrow use case, and offers a great deal of opportunities to improve upon it. 

## III. DevOps Motivation
Exposing a system to a real user base exposes major differences between software development and operations: Reproducible builds, automated deployments, and monitoring become essential to the system, as these allow system operators to detect and address issues.

These issues are commonplace enough that many modern applications are designed as complex multi-service pieces of software, with accordance with the principles of the 12 Factor App, as the current system design paradigm. As for system operations, the DevOps cycle has become the typical system operation model, and therefore a wide variety of tools exist to implement its components.

Moreover, more and more applications are being hosted in the cloud, as opposed to on-premise local servers. This allows companies to avoid managing servers entirely, instead managing virtual servers from any number of cloud providers.

This also includes many development tools, which will come in later when development on the bot itself continues.

## IV. The Modern Vision
RollBot aims for generality: any roleplay system should at the very least be easily implemented and added, and any server should be able to customize as many rules as possible.

On the infrastructure side, character and server data will be persistent by using a modern database solution, such as PostgreSQL. The service will run as a containerized bot, able to handle multiple Discord server at once. It will be hosted on the cloud, and scale for usage.

Finally, campaign management will benefit from a web interface, instead of being done via commands in Discord messages. 

## V. DevOps Roadmap
The following is an outline of the next steps in the project:

### i. Containerization
Wrapping the application in a Docker image allows for easy deployment and reproduction. Additionally, this will later be easily orchestrated as part of a Kubernetes cluster.

### ii. Persistence
Originally, RollBot stored character sheets locally as JSON files. Once again: this was a fine enough solution for the time, but does not hold up to modern standards; querying a local filesystem comes with unnecessary overhead issues. Discord servers and character sheets per system will be stored in their own database, allowing ease of access by multiple services.

### iii. CI Pipelines
As RollBot's complexity arises, it is bound to become less stable as a software product. CI, or Continuous Integration, ensures code changes are validated before being pushed into production. This validation step runs on every push to the main development branch, using GitHub Actions.

### iv. Cloud Deployment
At this step, RollBot is cloud-ready and migration should be seamless in theory. In practice, this step will likely be the roughest, as I expect to run into networking issues beyond my knowledge, which is exactly my hope (see part VII).

### v. Monitoring
Once RollBot runs on the cloud, resource usage becomes a critical bottleneck in operations. Toolkits like Grafana provide a suite of data collection and analysis tools, which enable further development and issue fixes.

To summarize:
> Discord
>    ↓
> RollBot Service
>    ↓
> PostgreSQL
>    ↓
> Monitoring Stack
## VI. Current Codebase
Currently on the repository a basic Python implementation of the bot can already be found. It uses Discord's Python API and contains a module for encapsulating roleplaying systems and their rules, with Dungeons and Dragons 5th Edition (DND5E) as an example of such a system. It is capable of holding character sheets and perform skill checks, possibly with Advantage or Disadvantage (taking the maximum or minimum of two dice rolls respectively).

## VII. RollBot as a DevOps Use Case
Before deciding on RollBot, I was planning on doing a simple "Hello world" project to get familiar with the cloud. However, that would create only superficial familiarity, not a deep understanding of real world development.

RollBot can have a real user base, generating real data and usage logs, as well as real user feedback. This user base may not necessarily be developers themselves, making user accessibility a major goal as well. This is another positive point for this project, as I believe a good system is a well communicated system, and a well communicated system can be understood regardless of technical background. 

## VIII. What Comes Next
Before starting infrastructure work, the codebase needs some final cleanups. The scope of the System abstract class is already well defined, and many of the important commands are already implemented. Next, containerization will allow running across a variety of hosts, which will naturally lead to a smoother migration to the cloud.

## IX. Conclusion
As I reach the following technical milestones, I will write more blogposts to cover the technical side of development and any insight I may have while developing. Future posts will cover CI implementation, containerization, and migration to the cloud.