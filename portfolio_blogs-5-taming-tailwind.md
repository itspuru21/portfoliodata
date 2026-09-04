---
title: "Portfolio Blog 5 Taming Tailwind CSS - Styling a Modular Static Site"
description: "Integrating Tailwind CSS with Vite in a modern frontend environment to give the portfolio a professional UI."
date: "2026-09-03"
---

With the repository structure successfully laid out in [Blog 4](#/blog/portfolio-blog-4-laying-groundwork), the backend foundation was solid. However, a decoupled architecture doesn't mean much if the user interface looks like a relic from the early 2000s. 

My original monolith relied on a single, massive `style.css` file. It was a nightmare to maintain. For this new phase, I wanted a professional, responsive UI, and [Tailwind CSS](https://tailwindcss.com/) was the obvious choice. But getting it to work cleanly within our modern Vite-powered frontend required setting up the right build pipeline.

## The Vite Build Pipeline

As you can see in our root directory, we are using **Vite** (`vite.config.js`) as our frontend build tool rather than a bulky legacy setup. Vite provides lightning-fast hot module replacement (HMR) and an optimized production build process.

To bring Tailwind into this environment, we didn't need a standalone configuration file cluttering the root. Instead, Tailwind integrates natively through PostCSS, scanning our components inside the `src/` directory automatically.

## Injecting the Tailwind Directives

The setup boils down to configuring our main entry stylesheet (`src/index.css`). By injecting Tailwind's core layers at the top of the CSS file, Vite's bundler processes and compiles utility classes seamlessly on every build:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

![Screenshot of your index.css file showing Tailwind directives](/portfolio/images/blog5A.png)
*(Caption: Configuring Tailwind inside our `src/index.css` entry point for Vite.)*

## Seamless CI/CD Integration

Because Vite handles the bundling during build time, our GitHub Actions pipeline doesn't need any special standalone CLI watchers. When the workflow runs `npm run build`, Vite automatically compiles our optimized CSS alongside our React components and outputs everything cleanly for production.

With the CSS pipeline established, I finally had a beautiful, responsive canvas. In [Blog 6](#/blog/portfolio-blog-6-interactive-ui), I will explain how I used this styling to build the interactive UI components—like the expandable project cards—using React and clean state management.