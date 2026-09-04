---
title: "Portfolio Blog 9 The Crazy Idea - Using GitHub as a Headless Database"
description: "Conceptualizing the backend architecture: How to leverage the GitHub REST API to perform CRUD operations without a traditional database."
date: "2026-09-03"
---

At this point in the build, the public-facing portfolio was complete. It could dynamically fetch, parse, and filter Markdown files, presenting them in a highly polished UI. But I still had a major operational bottleneck: I had to write those Markdown files locally in VS Code and push them to the repository via the command line.

If I was going to use this portfolio to document my journey into AWS and Kubernetes, I needed a frictionless publishing experience. I needed a Content Management System (CMS) like WordPress or Ghost.

However, adding a traditional CMS would destroy the elegant, database-less GitOps architecture I had spent [the last several days designing](#/blog/2026-09-03-portfolio-blog-3-the-pivot-to-gitops-designing-a-database-less-architecture). It would require a backend server, a database instance, and constant maintenance.

I needed a CMS that didn't require a backend. I needed a headless CMS. And then I realized: **I already have one.**

## The GitHub REST API

GitHub is not just a version control platform; it is a massively powerful, highly available, document-based database with a native REST API. 

If you think about it from a data architecture perspective:
*   A **Repository** is a Database.
*   A **Directory** (like `/content/blogs`) is a Table or Collection.
*   A **File** (like `my-post.md`) is a Record or Document.

The GitHub REST API allows you to perform full CRUD (Create, Read, Update, Delete) operations on repository contents. You can send a `PUT` request with base64-encoded text to the API, and GitHub will automatically create a new file and generate a commit for you. 

## The Blueprint for the Bespoke CMS

This realization changed everything. I didn't need to provision a Postgres database on AWS or spin up a Node backend. I just needed to build a frontend application capable of authenticating with the GitHub API. 

The blueprint was simple but ambitious:
1.  Build an isolated, secure application within the `/admin` directory we created in [Blog 4](#/blog/2026-09-04-portfolio-blog-4-laying-the-groundwork-structuring-for-scalability).
2.  Provide a rich text editor where I can write blog posts natively in Markdown.
3.  On "Publish," take that content, bundle it with YAML metadata, encode it, and hit the GitHub API.
4.  Let GitHub handle the actual storage, versioning, and triggering of the CI/CD deployment pipeline.

This is the ultimate expression of the "static-dynamic" hybrid approach. In [Blog 10](#/blog/2026-09-04-portfolio-blog-10-building-the-bespoke-cms-react-as-an-admin-panel), I will break down exactly how I built this internal tool using React, and how it securely interfaces with the repository.