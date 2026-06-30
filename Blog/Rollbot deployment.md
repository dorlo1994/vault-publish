---
title: "RollBot Deployed! What's next?"
date: 2026-06-30
type: blog
tags: [devops, saas, tabletop_roleplay]
draft: false
---
## I. From Script to Service

When I first wrote RollBot back in 2020, it was exactly what it needed to be: a small JavaScript program running on my laptop. It stored character sheets for my Dungeons & Dragons group and automated dice rolls during our online sessions. It solved the immediate problem, and when that campaign ended, so did the project.

Several years later, I revisited the idea—not because the original implementation was inadequate, but because the project had become an ideal vehicle for learning cloud-native software engineering. Rather than treating deployment as an afterthought, I wanted infrastructure to become part of the design process itself.

---

## II. One Application, Four Services

The original RollBot was a single process responsible for everything. That was perfectly reasonable at the time, but as the project grew, it became clear that different responsibilities belonged in different components.

Today, RollBot consists of four services:

- A Discord client responsible for interacting with Discord.
- A FastAPI service exposing the application's data and business logic.
- A PostgreSQL database providing persistent storage.
- A React dashboard for managing guilds, channels and characters through a web interface.

Each service is independently containerized and communicates over Docker's internal network.

This separation wasn't driven by scale—RollBot certainly doesn't need Kubernetes—but by maintainability. Separating concerns makes each component easier to reason about, test and eventually replace.

---

## III. Designing for Deployment

One of the more interesting changes wasn't adding a new feature, but removing assumptions.

The original application assumed everything lived on one machine. Once multiple containers entered the picture, that stopped being true.

That meant introducing:

- service-specific configuration through environment variables,
- a reverse proxy so the frontend no longer needed to know where the API lived,
- health checks so dependent services waited for one another,
- persistent Docker volumes for PostgreSQL,
- and a deployment process that can reproduce the same environment on any machine.

None of these change what RollBot does for its users. They change how reliably it can be operated.

---

## IV. First Cloud Deployment

This week, I deployed RollBot to AWS for the first time.

Like most first deployments, it involved a fair amount of debugging:

- container networking,
- database initialization,
- reverse proxy configuration,
- Docker health checks,
- environment management,
- and the inevitable security group misconfiguration.

Each issue reinforced an important lesson: deploying software is less about following a checklist and more about understanding how the different layers of the system interact.

---

## V. What's Next?

The deployment marks an important milestone, but not the end of the project.

The next goals are to automate what is currently manual:

- API tests,
- GitHub Actions,
- automated deployments,
- HTTPS,
- monitoring and observability.

The application itself will continue to evolve as well, with support for additional tabletop systems and richer campaign management.

For me, RollBot has become more than a Discord bot. It's a continuously evolving software project where product design, backend development and infrastructure all influence one another—and that's precisely what makes it an interesting platform for learning.