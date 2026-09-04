---
title: "Portfolio Blog 15 When Actions Fail - Debugging CI CD"
description: "Embracing the DevOps mindset by documenting the pipeline failures, permission errors, and infinite loops encountered while automating deployments."
date: "2026-09-03"
---

In [Blog 14](#/blog/portfolio-blog-14-pipeline-magic), I outlined the elegant CI/CD pipeline I built using GitHub Actions to automatically compile my Tailwind CSS and deploy the site. 

It sounds perfect on paper. But any seasoned Cloud or DevOps engineer will tell you the truth: pipelines almost never work on the first try. Writing the YAML file is only half the battle; debugging it is the rest.

I want to use this post to document the two major failures I encountered while building this automation, and the solutions that finally turned that red "X" into a green checkmark.

## Failure 1: The Permission Denied Error

My first workflow run failed spectacularly during the final deployment step. The GitHub runner successfully checked out the code, installed Node.js, and built the CSS. But when it tried to push the compiled assets to the `gh-pages` branch, it threw a fatal `403 Forbidden` error.

**The Root Cause:** By default, GitHub Actions runners only have read access to the repository for security reasons. The pipeline was trying to push a new commit, but it wasn't authorized.

**The Fix:** I had to explicitly grant the `GITHUB_TOKEN` write permissions within the workflow YAML file. 

```yaml
# The missing piece of the puzzle
permissions:
  contents: write
```
Once I added this block to my `.github/workflows/deploy.yml`, the runner was authorized to push the compiled artifacts.

![Screenshot of a failed GitHub Actions log](/portfolio/images/blog15A.png)
*(Caption: Digging into the terminal logs to find the root cause of the deployment failure.)*

## Failure 2: The Infinite Loop

Once the permissions were fixed, the site deployed successfully! I used the React CMS to publish a test post, which triggered the Action, which deployed to the `gh-pages` branch. 

But then, the Action triggered *again*. And again. I had accidentally created an infinite deployment loop.

**The Root Cause:** My trigger was set to run `on: push`. When the Action successfully deployed, it pushed a new commit to the `gh-pages` branch. Because a push occurred, GitHub triggered the Action again, leading to an endless cycle.

**The Fix:** I needed to strictly scope the workflow trigger so it would ignore the `gh-pages` branch entirely. 

```yaml
# Scoping the trigger to only the source code branch
on:
  push:
    branches:
      - main
```

## The Value of Failure

These failures weren't setbacks; they were the most valuable part of the build. In DevOps, configuring an environment correctly the first time is rare. The real skill is reading the logs, understanding the underlying system permissions, and applying a precise fix. 

This concludes Phase 1 of the portfolio project. I now have a fully modular, GitOps-driven, serverless Markdown architecture with an automated deployment pipeline. 

But as I stated back in Day 1, this is just a stepping stone. In [Blog 16](#/blog/portfolio-blog-16-phase-1-complete), I will summarize what we achieved and outline the blueprint for Phase 2: escaping GitHub Pages and migrating this entire architecture to AWS EKS using Terraform!