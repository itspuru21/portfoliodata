---
title: "Portfolio Blog 11 API Acrobatics and Asset Management"
description: "Overcoming GitHub API limitations, managing authentication, and structuring our public directory to handle static images efficiently."
date: "2026-09-03"
---

Building the React CMS to push Base64-encoded Markdown directly to the repository felt like a massive victory. However, treating a version control platform like a headless database is bound to introduce some edge cases. 

When you bypass a traditional backend, you inherit a new set of architectural challenges: specifically, how do you handle security, and how do you manage binary files like images?

## Securing the API

Since our React CMS operates entirely in the browser, there is no secure backend vault to store API keys. If I hardcoded my GitHub Personal Access Token (PAT) into the React app, anyone inspecting the network tab could steal it and gain write access to my repository. 

The workaround? The CMS relies on a localized environment variable during my local development, or a secure browser `localStorage` prompt that asks for the token strictly on my own machine. The token is never committed, and the static files deployed to GitHub Pages remain completely ignorant of the authentication process.

## The Asset Management Decision

With text publishing secured, I had to decide how to handle images. I needed to include architectural diagrams and screenshots in these blog posts. 

In a traditional cloud setup, this is where you provision an AWS S3 bucket. Some developers also use external CDNs or image hosting hacks to keep their repository size small. However, for Phase 1, I decided to prioritize **strict version control and atomic deployments**.

Instead of outsourcing image hosting, I chose to store all media directly inside the `public/images/` folder of the repository. 

```text
📁 my-devops-portfolio
 ├── 📁 content
 └── 📁 public
      └── 📁 images
           ├── 📄 aws-architecture.png
           └── 📄 terminal-error.png
```

![Screenshot of the public/images directory](/portfolio/images/blog11A.png)
*(Caption: Keeping static assets self-contained within the repository.)*

## Why This Works for Now

When I write a blog post in the CMS, I simply reference the relative path (e.g., `/portfolio/images/aws-architecture.png`). 

From a DevOps perspective, keeping images in the repository has a distinct advantage: **State consistency**. The blog post and the image it references are tied to the exact same Git commit. If I roll back the repository to a state from a month ago, the images roll back perfectly with it. There are no broken links caused by an external S3 bucket changing. 

With content and media perfectly handled and self-contained, there was only one piece of the frontend puzzle left: keeping everything synchronized. In [Blog 12](#/blog/2026-09-04-portfolio-blog-12-state-and-syncing-forcing-a-static-site-to-act-dynamic), I will break down how we forced the static frontend to stay perfectly in sync with the rapid updates coming from our React CMS.