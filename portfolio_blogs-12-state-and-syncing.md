---
title: "Portfolio Blog 12 State and Syncing - Forcing a Static Site to Act Dynamic"
description: "Tackling cache invalidation and synchronization delays between the React CMS and the vanilla JavaScript frontend."
date: "2026-09-03"
---

At this point, we have a fully functional content pipeline. We can write Markdown in our React CMS, manage our static assets directly within the repository's public folder, and push the final payload directly to the repository via the GitHub REST API. 

But a hidden engineering challenge immediately surfaced once I started testing this flow: **Synchronization and Caching**.

## The Propagation Delay

When a traditional backend saves a blog post to a database, it is instantly available for the next user who refreshes the page. 

Our GitOps architecture works differently. When the React CMS pushes a new `.md` file to the repository, it triggers a GitHub Action to rebuild and deploy the GitHub Pages site. This process isn't instantaneous—it takes about 60 to 90 seconds. 

If I hit "Publish" and immediately navigate to my public portfolio, the new blog post won't be there. From a user experience perspective, this is confusing. Did the API call fail? Did the deployment crash?

## Polling the CI/CD Pipeline

To solve this, I needed the React CMS to be aware of the GitHub Actions environment. 

I updated the CMS logic so that immediately after a successful `PUT` request, it enters a "Deploying" state. It then begins polling the GitHub Actions API every 10 seconds, checking the status of the latest workflow run on the `main` branch. 

Only when the API returns a `status: "completed"` and `conclusion: "success"` does the CMS UI flash a green success message and allow me to navigate to the live site. 

![Polling GitHub Actions Demo](https://raw.githubusercontent.com/itspuru21/portfoliodata/main/images/blog12A.gif)
*(Video: The React CMS polling the GitHub Actions API to track deployment state.)*

## The Browser Cache Invalidation

The second hurdle was the browser itself. In [Blog 7](#/blog/2026-09-04-portfolio-blog-7-the-markdown-engine-rendering-content-without-a-server), we built a vanilla JavaScript engine that fetches our metadata and Markdown files. Browsers love to aggressively cache static files like `.json` and `.md` to save bandwidth.

Even if GitHub Pages successfully deployed the new content, my browser would often load the old, cached version of the blog list. 

To force the browser to act dynamically, I implemented a classic cache-busting technique. I modified the JavaScript `fetch()` calls to append a unique timestamp query parameter to the end of the file request:

```javascript
// Forcing the browser to fetch the freshest data
const timestamp = new Date().getTime();
fetch(`/content/blogs/metadata.json?t=${timestamp}`)
  .then(response => response.json());
```

Because the URL is technically different every single millisecond, the browser is forced to bypass its cache and pull the absolute latest state directly from the GitHub Pages server.

With state and syncing fully resolved, Day 3 of the build was complete. The CMS was robust, secure, and fully synchronized with the frontend. 

Now, it is time to move on to Day 4: injecting truly dynamic features into our static site. In [Blog 13](#/blog/2026-09-04-portfolio-blog-13-the-database-less-counter-tracking-hits), I will break down how I built an automated visitor counter without using a database!