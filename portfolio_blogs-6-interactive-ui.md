---
title: "Portfolio Blog 6 The Interactive Static UI - A DevOps Approach to Frontend"
description: "Building expandable project cards and interactive UI elements using pure JavaScript, applying an engineering mindset to frontend development."
date: "2026-09-03"
---

Let me start with a confession: I am not a frontend developer. My passion lies in Cloud infrastructure, DevOps pipelines, and automation. 

However, presenting complex architectural diagrams and CI/CD workflows requires a clean, readable interface. While I am immensely proud of the UI I built for this project, I approached its creation not as a web designer, but as an engineer. I wanted the frontend to be as modular, efficient, and lightweight as the infrastructure I plan to host it on.

## Engineering the Expandable Cards

One of the key features I wanted was expandable project cards. I needed a way to show a high-level summary of a project (like the tech stack and a quick blurb) and allow the user to click to reveal the deep technical dive, without navigating to a new page.

In a framework like React, this is a trivial state management task. But in our [purely static, database-less architecture](#/blog/portfolio-blog-3-pivot-to-gitops), I had to rely on vanilla JavaScript. 

Instead of writing repetitive code for every single card, I treated the UI components like modular infrastructure. I wrote a single, reusable JavaScript function that dynamically targets data attributes (`data-target`) on the HTML elements to toggle the hidden classes we generated with Tailwind in [Blog 5](#/blog/portfolio-blog-5-taming-tailwind).

<iframe src="https://drive.google.com/file/d/YOUR_FILE_ID/preview" className="w-full aspect-video rounded-xl shadow-lg my-4" allow="autoplay"></iframe>
*(Video: Demonstrating the lightweight, vanilla JS expandable project cards.)*

## Modularity Over Complexity

The logic here mirrors good DevOps practices:
*   **DRY (Don't Repeat Yourself):** One JavaScript event listener handles all cards dynamically.
*   **Decoupled State:** The visual styling is strictly handled by CSS classes, while JS only handles the binary state (open/closed).
*   **Zero Dependencies:** No heavy libraries like jQuery or React were downloaded just to toggle a `div`. 

By keeping the frontend logic barebones, I ensure the site loads instantly. The browser doesn't have to parse megabytes of vendor code before rendering the page.

With the visual shell and the basic interactivity now in place, the stage is set for the real technical heavy lifting. The UI is just an empty container. In [Blog 7](#/blog/portfolio-blog-7-markdown-engine), I will dive into the core of the GitOps engine: how we actually fetch, parse, and inject raw Markdown files into these beautifully styled cards.