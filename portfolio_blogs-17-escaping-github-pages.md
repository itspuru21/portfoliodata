---
title: "Portfolio Blog 17 Escaping GitHub Pages - The Case for AWS"
description: "Analyzing the limitations of managed static hosting and outlining the strategic pivot toward enterprise-grade AWS infrastructure."
date: "2026-09-03"
---

Our Phase 1 GitOps architecture has been a tremendous success. We have a bespoke React CMS, a client-side Markdown engine, and a fully automated GitHub Actions pipeline. 

For a traditional web developer, this would be the finish line. GitHub Pages is hosting the site for free, serving it over a global CDN, and automatically managing the SSL/TLS certificates. 

But for a Cloud and DevOps engineer, a fully managed service is a double-edged sword. It is time to escape GitHub Pages and move to AWS. Here is why.

## The "Black Box" Problem

The biggest advantage of GitHub Pages is also its biggest flaw for this project: **Abstraction**. 

When I push code to the `gh-pages` branch, GitHub handles the deployment, networking, load balancing, and scaling invisibly. It is a "Black Box." 

As I prepare for roles in DevOps and Cloud Architecture, my primary job will be to design, build, and maintain the very infrastructure that GitHub is currently hiding from me. I cannot demonstrate my ability to configure Virtual Private Clouds (VPCs), manage EC2 instances, or orchestrate containers if I am relying on a platform that does it all for me.

## The Containerization Mandate

To move off GitHub Pages, the application itself needs to change. Right now, it is just a collection of static files sitting in a repository branch. 

To host this on AWS, we must introduce **Docker**. By containerizing the frontend web server (likely using NGINX) and our React CMS, we ensure that our application can run identically on my local laptop, a single AWS EC2 instance, or a massive Kubernetes cluster. 

![Conceptual diagram: GitHub Pages vs AWS VPC](/portfolio/images/blog17A.png)
*(Caption: Moving from a managed black box to a self-architected Cloud environment.)*

## Why EKS?

I could just spin up a single AWS EC2 instance, install Docker, and run the container. But that is fragile. If that single EC2 instance goes down, the portfolio goes down. 

In the enterprise world, high availability and self-healing are non-negotiable. This is why the ultimate destination for Phase 2 is **Amazon Elastic Kubernetes Service (EKS)**. 

Migrating this static-dynamic site to Kubernetes might seem like massive overkill—and for a simple blog, it absolutely is. But this is not just a blog; it is a live demonstration of my engineering capabilities. Deploying this architecture on EKS proves I can handle container orchestration, manage node groups, and configure ingress controllers.

In [Blog 18](#/blog/2026-09-04-portfolio-blog-18-aws-blueprint-designing-the-cloud-architecture), we will translate this strategy into a concrete architectural blueprint, detailing exactly how our VPC, Subnets, and EKS clusters will be laid out in the AWS Cloud.