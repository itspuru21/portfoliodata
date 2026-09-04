---
title: "Portfolio Blog 2 Hitting the Ceiling - Why the Monolith Had to Die"
description: "Exploring the growing pains of a purely static portfolio and the manual nightmare of updating content without a CMS."
date: "2026-09-03"
---

In my previous post, I talked about the simplicity of my original 3-file portfolio setup. It was fast, easy to deploy, and required zero maintenance. But as I dove deeper into Cloud and DevOps engineering, the honeymoon phase quickly ended. 

I needed a space to document my journey—to write about AWS, Kubernetes, Terraform, and the various errors I was debugging. I needed a blog. And that is exactly where the monolithic architecture hit a brick wall.

## The Nightmare of Manual Content Management

When you don't have a backend or a Content Management System (CMS), your content and your code are deeply intertwined. 

Here is what the process looked like every time I wanted to add a new project or blog post:
1.  Open `index.html` in my code editor.
2.  Find the exact line where the previous project ended.
3.  Copy and paste a massive block of HTML `<div>` tags to create a new card.
4.  Manually swap out the text, image links, and URLs directly in the code.
5.  If it was a new blog page, I had to create a brand new `.html` file and then manually update the navigation links on *every single other page* so they could link to it.

```text
<!-- The reality of hardcoding a blog list -->
<div class="blog-list">
  <a href="blog-1.html">Blog 1: Humble Beginnings</a>
  <a href="blog-2.html">Blog 2: Hitting the Ceiling</a> <!-- I had to manually type this! -->
</div>
```

![Screenshot showing messy HTML code with repetitive div blocks](/portfolio/images/blog2A.png)
*(Caption: The raw HTML file—where content updates meant risky code edits.)*

## The Automation Gap

Beyond the sheer annoyance of copy-pasting HTML, this setup violated a core principle of the career I am pursuing. 

As an aspiring DevOps engineer, my days are spent learning how to automate infrastructure, build CI/CD pipelines, and separate configuration from code. Yet, my personal portfolio—the very thing meant to showcase my skills—was entirely manual. 

*   There was no **Continuous Integration**. 
*   There was no **version control for content** (only for code).
*   There was no **dynamic rendering**.

## The Pivot

I realized I couldn't just keep piling HTML files into a repository. But I also didn't want to lose the benefits of hosting on GitHub Pages (it's free, fast, and reliable). I didn't want to spin up a heavy backend database just to serve text.

I needed a hybrid. I needed the performance of a static site, but the developer experience of a dynamic CMS. 

In [Blog 3](#/blog/portfolio-blog-3-pivot-to-gitops), I will outline the architectural pivot: how I designed a database-less, GitOps-driven architecture that allows me to write in Markdown and let the code do the rest.