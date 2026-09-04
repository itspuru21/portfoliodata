---
title: "Portfolio Blog 1 The Humble Beginnings - My Original 3-File Portfolio"
description: "A look back at where it all started: a simple, static HTML/CSS/JS monolith, and why it was the perfect starting point before evolving."
date: "2026-09-03"
---

Every engineer has to start somewhere. Before Kubernetes clusters, CI/CD pipelines, and infrastructure as code, there was usually just a folder on a desktop containing a few basic files. 

For me, that starting point was a monolithic, entirely static portfolio website. As I sit here writing this first post on my newly engineered, GitOps-driven Markdown CMS, I want to take a look back at where this project began: the classic 3-file setup.

## The Architecture of "Version 0"

When I first decided to put my B.Tech Computer Science achievements online, I didn't overthink the architecture. The goal was simple: get my name, my resume, and a few projects on the internet. 

The entire project consisted of exactly three files:
*   `index.html` - The skeleton and content.
*   `style.css` - The visual design.
*   `script.js` - A tiny bit of logic for basic interactions.

```text
📁 my-portfolio
 ├── 📄 index.html
 ├── 📄 style.css
 └── 📄 script.js
```

![Screenshot of my original 3-file portfolio website](/portfolio/images/blog1A.png)
*(Caption: A look back at the original static portfolio.)*

## Why It Worked (For a While)

There is a certain beauty in simplicity. This setup had zero build steps, zero dependencies, and absolutely no backend. 

1.  **Instant Deployment:** I could drag and drop it into GitHub Pages and it was live in seconds.
2.  **Lightning Fast:** Browsers parse raw HTML/CSS incredibly quickly.
3.  **No Maintenance:** There were no packages to update or vulnerabilities to patch.

It did exactly what a traditional web developer needed it to do. But there was one major problem: I am not aiming to be a traditional web developer. I am building toward a career in Cloud and DevOps Engineering.

## Hitting the Monolith's Ceiling

As I started learning AWS (EC2, EKS), Terraform, and Kubernetes, I wanted to write about them. I wanted to share my workarounds, my scripts, and my architectural diagrams. 

I quickly realized the 3-file monolith was a nightmare to scale:
*   **No CMS:** Every time I wanted to add a project, I had to manually write HTML `<div>` tags. 
*   **No Blogging Capability:** Creating a blog meant creating a new HTML page for every post and manually updating navigation links.
*   **No Automation:** It was a purely manual process that didn't showcase any of the DevOps practices I was actually learning.

It became clear that my portfolio itself needed to become a DevOps project. The architecture had to reflect my skills. 

In the next post, I’ll break down exactly why this purely static monolith had to die, and the initial blueprint for replacing it with a database-less, static-dynamic architecture.