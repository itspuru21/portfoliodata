---
title: "Portfolio Blog 13 The Database-less Counter - Tracking Hits"
description: "Implementing a dynamic visitor counter on a static site, understanding its architectural weaknesses, and prioritizing engineering honesty."
date: "2026-09-03"
---

As we transition into Day 4 of the build, the focus shifts to injecting dynamic features into our purely static GitHub Pages environment. The first feature on the list: a visitor counter.

Adding a counter to a static site usually requires setting up a database to increment a number every time the page loads. Since our GitOps architecture lacks a traditional backend, I had to find a serverless workaround using a free counting API to fetch and display the number dynamically via JavaScript.

But as an engineer, transparency is critical. Before I explain how I implemented this, I want to address the elephant in the room: **this counter has a massive architectural weakness.**

## The Flaw: Hits vs. Unique Visitors

In enterprise monitoring and observability, precision is everything. You need to know exactly *who* is doing *what*. 

The counter on this portfolio is **not** an analytics engine. It does not track unique IP addresses, nor does it drop session cookies to verify if a user has visited before. It is a simple "hit" counter triggered by a JavaScript API call every time the `index.html` file loads. 

This means if you land on the page, the counter goes up by 1. If you hit "Refresh" continuously 10 times, the counter goes up by 10, even though you are only one visitor. 

## Aesthetics Over Analytics

Why include a flawed counter at all? Because I am not trying to fake impressions or artificially inflate my traffic. If I wanted real metrics, I would implement Google Analytics or a proper CloudWatch logging stack.

The visitor counter is purely for aesthetics. It serves as a visual proof-of-concept that this static architecture can successfully make external API calls, handle asynchronous data fetching, and update the UI dynamically without a backend server. It is a frontend trick, not a data metric.

![Screenshot of the visitor counter on the UI](/portfolio/images/blog13A.png)
*(Caption: The dynamic counter UI—fetching data on the fly, even if it is just counting raw page hits.)*

## Implementing the Endpoint

To build this, I used a lightweight, open-source serverless counting API. When the React UI mounts, a vanilla JavaScript `fetch()` request hits the API endpoint, which automatically increments the stored integer by one and returns the new value. 

```javascript
// A simplified look at the raw hit counter logic
fetch('[https://api.countapi.xyz/hit/my-devops-portfolio/visits](https://api.countapi.xyz/hit/my-devops-portfolio/visits)')
  .then(res => res.json())
  .then(data => {
    document.getElementById('visitor-count').innerText = data.value;
  });
```

By keeping the scope of this feature strictly aesthetic, I avoided over-engineering a complex session-tracking system for Phase 1 of this project. 

With the dynamic UI elements out of the way, it is time to look at the automation holding this entire repository together. In [Blog 14](#/blog/portfolio-blog-14-pipeline-magic), we will dive into the GitHub Actions CI/CD pipeline!