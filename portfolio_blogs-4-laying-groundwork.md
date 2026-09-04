---
title: "Portfolio Blog 4 Laying the Groundwork - Structuring for Scalability"
description: "Transitioning from theory to practice by setting up the foundational repository structure for a modular, GitOps-driven portfolio."
date: "2026-09-03"
---

With the concept of a static-dynamic hybrid architecture firmly in place ([see Blog 3](#/blog/portfolio-blog-3-pivot-to-gitops)), it was time to actually start writing code. 

The goal for this phase was to set up a repository that cleanly separated the frontend presentation from the backend content. I needed a structure that would not only work for GitHub Pages today, but could also be easily containerized for an eventual migration to AWS EKS. 

## Ditching the Flat Directory

The old setup was a flat directory containing exactly three files ([see Blog 1](#/blog/portfolio-blog-1-humble-beginnings)). That lack of organization is fine for a weekend project, but it is a disaster for a maintainable codebase. 

I initialized a fresh Git repository and designed a strict directory hierarchy:

```text
📁 my-devops-portfolio
 ├── 📁 .github/workflows   # For our future CI/CD pipelines
 ├── 📁 admin               # The bespoke React CMS will live here
 ├── 📁 content
 │    ├── 📁 blogs          # Raw Markdown files go here
 │    └── 📁 projects       # JSON or Markdown project data
 ├── 📁 public              # Static assets (images, icons)
 └── 📁 src                 # Frontend UI components and logic
```

![Screenshot of the new directory structure in VS Code](/portfolio/images/blog4A.png)
*(Caption: The new, decoupled repository structure.)*

## Why This Structure Matters

This layout isn't just about keeping things tidy; it is a fundamental shift in how the application operates:

1.  **Strict Decoupling:** The `/src` directory (the UI) doesn't care *what* is inside the `/content` directory. It is just built to read whatever files exist there. This means I can add, delete, or modify blog posts without ever touching the UI code again.
2.  **Isolated CMS:** Reserving a dedicated `/admin` folder sets the stage for the custom React CMS I planned to build. It acts almost like a separate micro-application living within the same repository.
3.  **DevOps Readiness:** By isolating the configuration (`.github`), content (`content`), and application code (`src`), I am preparing this project for Dockerization. When the time comes for Phase 2, wrapping this structure in a container will be a straightforward process.

## The First Commit

Running `git init` and making that first initial commit felt like stepping into a new era of development. I was no longer just building a webpage; I was scaffolding an infrastructure.

But a great backend structure doesn't mean much if the frontend looks terrible. In [Blog 5](#/blog/portfolio-blog-5-taming-tailwind), I will dive into the UI layer and explain how I tackled the challenges of integrating Tailwind CSS into a purely static architecture to give this project a professional polish.