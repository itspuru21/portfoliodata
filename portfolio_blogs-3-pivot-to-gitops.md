---
title: "Portfolio Blog 3 The Pivot to GitOps - Designing a Database-less Architecture"
description: "How I conceptualized a static-dynamic hybrid architecture using Git as a headless database to prepare for future cloud migrations."
date: "2026-09-03"
---

The monolithic HTML site was dead to me. It was time to architect a system that actually reflected my goals in Cloud and DevOps. 

I knew the end state for this project would eventually be a highly available cluster running on AWS EKS, provisioned by Terraform. But jumping straight from three hardcoded files to a full Kubernetes deployment was a massive leap. I needed an intermediate step—a "Phase 1" architecture that solved my immediate problem (content management) while setting the foundation for the cloud.

## The Architectural Requirements

I laid out three core requirements for the new portfolio:
1.  **Separation of Concerns:** Content (blogs, project data) must be completely decoupled from the UI code. 
2.  **No Traditional Backend:** I wanted to stick to GitHub Pages for hosting, which meant I couldn't run a Node.js or Python backend server, nor connect to a traditional SQL database.
3.  **Automation First:** Deployments and content updates had to be automated via CI/CD.

## The GitOps Epiphany

If I couldn't use a traditional database, where would my content live? The answer was staring me in the face: **Git**.

GitOps is a practice where a Git repository serves as the single source of truth for declarative infrastructure and applications. I decided to apply this concept directly to my content. 

Instead of storing blog posts in a Postgres database, I would store them as raw Markdown (`.md`) files in the repository. Instead of a backend server fetching data on every page load, the frontend would pull these files at build time (or fetch them statically on the client side).

![Architectural diagram showing GitOps workflow](/portfolio/images/blog3A.png)
*(Caption: The Static-Dynamic Flow: Using Git as a headless CMS.)*

## The "Static-Dynamic" Hybrid

This approach gave birth to what I call my static-dynamic architecture:

*   **The Database:** A structured directory of Markdown files and JSON metadata living in the GitHub repository.
*   **The Engine:** A modern frontend setup that knows how to parse these static files and render them beautifully.
*   **The Pipeline:** GitHub Actions acting as the CI/CD pipeline, taking my Markdown commits and automatically deploying the updated site.

This setup is infinitely scalable for a personal site. If I write 100 blog posts, I just commit 100 Markdown files. No database queries to optimize, no backend endpoints to secure.

With the conceptual architecture locked in, it was time to actually start building. In [Blog 4](#/blog/portfolio-blog-4-laying-groundwork), I will break down how I laid the initial groundwork and structured the repository for this new modular era.