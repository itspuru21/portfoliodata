---
title: "Portfolio Blog 21 The Meta-Blog - Engineering the Blogging Engine"
description: "A behind-the-scenes look at the failures, pivots, and technical roadblocks we hit while building a custom GitOps Markdown CMS."
date: "2026-09-03"
---

Building a portfolio to document your DevOps journey is one thing. Building a custom, headless GitOps Content Management System (CMS) from scratch to host it is entirely another. 

While the previous 20 posts detail the clean, final architecture of this project, they leave out the gritty reality of software engineering: **the failures**. 

This is the meta-blog. This is the story of how the blogging engine itself was built, the architectural dead-ends I ran into, and the workarounds I engineered to fix them.

## Failure 1: The React and GitHub Pages Routing Trap

Once the basic Markdown engine was fetching text, I tried to add my first image. I dragged an image into my VS Code `/public/images/` folder and wrote a standard Markdown tag: `![My Image](/public/images/imgAa.png)`. 

It resulted in a broken image icon. I was fighting two distinct build systems at once. Vite strips the `/public/` folder during the build, and GitHub Pages (because it hosts on a sub-path like `/portfolio/`) requires the repository name in the URL. 

The fix was stripping `/public/` and manually injecting the repository name: `/portfolio/images/imgAa.png`. But this immediately led to a much bigger architectural realization.

## Failure 2: The "Same Repo" Storage Bottleneck

I had just spent days building a seamless, headless React CMS so I would never have to touch my code editor to publish a blog post. But I quickly realized a fatal flaw: pushing an `.md` file via the React CMS API didn't magically push the local binary image files to GitHub. 

If I kept storing images in the same repository (`/public/images/`), I still had to manually drag files into my IDE, run `git add`, `git commit`, and `git push` every single time I wanted to add media to a post. It completely defeated the purpose of the automated CMS. I was still touching the code repository just to upload pictures. 

I needed a decoupled storage solution. 

## Failure 3: The GitHub Issues CDN Exploit

To decouple my images without paying for an AWS S3 bucket, I considered a classic developer hack: using GitHub Issues as a personal image storage CDN. 

If you paste an image into a dummy GitHub Issue, GitHub uploads it to their own backend and gives you a free `user-images.githubusercontent.com` URL. You can just copy that URL and paste it into your blog. 

But as an engineer, this felt wrong. It is an exploit, not an architecture. Relying on an undocumented loophole is a severe DevOps anti-pattern. Eventually, GitHub is going to fix this, patch the exploit, or periodically clear out orphaned assets from Issues. If that happens, every single image on my portfolio would instantly break. I needed a legitimate, stable external storage solution.

## Failure 4: The Google Drive Hotlink Block

I pivoted to Google Drive. It was free, decoupled from my code, and legitimate. I uploaded my assets and generated direct download links using the `uc?export=view` format. 

But Google aggressively blocks hotlinking to prevent people from using Drive as a free Content Delivery Network. The browser threw CORS (Cross-Origin Resource Sharing) and permission errors. 

**The Final Solution:** I had to pivot to an iframe embed architecture. Instead of standard Markdown image tags, I injected raw HTML into the Markdown using Google's official `/preview` endpoint, wrapped in Tailwind classes for responsiveness:
```html
<iframe src="[https://drive.google.com/file/d/YOUR_FILE_ID/preview](https://drive.google.com/file/d/YOUR_FILE_ID/preview)" className="w-full aspect-video rounded-xl shadow-lg" allow="autoplay"></iframe>
```
This solved everything. It bypassed the hotlink blocks, allowed seamless media streaming, and achieved zero repository bloat without relying on fragile exploits.

## Failure 5: The URL Slug Nightmare

To create a cohesive "Zettelkasten" digital garden, I needed cross-blog deep linking using hash-routing: `#/blog/my-awesome-post`. 

My JavaScript engine read the `title` from the YAML frontmatter to generate these slugs. However, my early blog titles looked like this: `title: 'Portfolio "Blog 7: The Markdown Engine – Rendering Content"'`.

URL parsers hate colons, nested quotation marks, and special characters. My auto-generated slugs were breaking the React HashRouter, causing links to fail silently. I had to implement strict data-sanitization rules, stripping all special characters from the YAML frontmatter entirely so the underlying slug generation became bulletproof.

## Engineering is Iteration

You rarely get the architecture right on the very first commit. Building this custom GitOps CMS forced me to confront edge cases in build tools, browser caching, and REST API limitations. 

The final product is my portfolio costing me nothing to keep it up and can later be easily deployed on the actual devops/cloud infrastructure very easily which we call migration and upgrad