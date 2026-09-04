---
title: "Portfolio Blog 14 Pipeline Magic - Automating the Build"
description: "Designing a GitHub Actions CI/CD pipeline to automate Tailwind compilation and GitHub Pages deployment."
date: "2026-09-03"
---

Throughout this build, we have escaped the nightmare of manual HTML edits, built a headless React CMS, and established a way to push Markdown via the GitHub API. 

But there is a missing link. When I hit "Publish" in the CMS, the repository updates with a new `.md` file, but the live website doesn't magically change on its own. It needs to be built and deployed. 

As someone aiming for a career in Cloud and DevOps, manually running build commands and uploading files is an anti-pattern. Everything must be automated. 

## The CI/CD Mindset

Continuous Integration and Continuous Deployment (CI/CD) is the backbone of modern infrastructure. For this Phase 1 portfolio, I utilized **GitHub Actions** to serve as my CI/CD pipeline. 

The goal was to create a workflow that listens for any changes to the `main` branch (like when my React CMS pushes a new blog post) and automatically handles the heavy lifting.

Here is the exact flow I engineered into my `.github/workflows/deploy.yml` file:

1.  **The Trigger:** The pipeline initiates `on: push` to the `main` branch.
2.  **Environment Setup:** It provisions an Ubuntu runner and sets up a Node.js environment.
3.  **The Build Step:** Remember our Tailwind CSS setup from [Blog 5](#/blog/2026-09-04-portfolio-blog-5-taming-tailwind-css-styling-a-modular-static-site)? The pipeline runs `npx tailwindcss -i ./src/input.css -o ./public/output.css --minify` to generate a fresh, compressed stylesheet.
4.  **The Deployment:** Finally, it takes the newly compiled CSS, along with all my HTML, JS, and Markdown files, and forces a deployment to a separate branch called `gh-pages`.

![Screenshot of a successful GitHub Actions run](/portfolio/images/blog14A.png)
*(Caption: The CI/CD pipeline in action, automatically building and deploying the portfolio.)*

## The Beauty of the gh-pages Branch

Deploying to a dedicated `gh-pages` branch is a classic GitOps pattern for static sites. 

By configuring GitHub Pages to only serve files from this specific branch, I keep my `main` branch clean. My `main` branch contains all the source code, React CMS logic, and raw Markdown. But the `gh-pages` branch only contains the final, compiled production assets. 

This strict separation of source code and compiled artifacts ensures my repository stays organized and scalable. 

With this pipeline, my publishing workflow is now completely frictionless. I write a post in the browser, hit publish, and the pipeline does the rest. 

But as any DevOps engineer knows, pipelines don't always run perfectly on the first try. In [Blog 15](#/blog/2026-09-04-portfolio-blog-15-when-actions-fail-debugging-ci-cd), I will document the CI/CD failures I encountered and how I debugged them.