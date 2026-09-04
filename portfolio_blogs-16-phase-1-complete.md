---
title: "Portfolio Blog 16 Phase 1 Complete - The Stepping Stone to the Cloud"
description: "A retrospective on the GitOps architecture built so far, and the blueprint for containerizing and migrating the project to AWS EKS."
date: "2026-09-03"
---

Over the past 15 posts, we have completely transformed this portfolio. What started as a brittle, 3-file HTML monolith is now a fully decoupled, GitOps-driven architecture. 

I built a bespoke React CMS that uses the GitHub REST API as a headless database, engineered a client-side Markdown rendering engine, and fully automated the deployments using GitHub Actions. 

Phase 1 was a massive success. It solved my immediate problem: giving me a frictionless platform to document my engineering journey. But as I look toward my ultimate goal of landing a Cloud or DevOps engineering role, Phase 1 is no longer enough.

## The Limits of GitHub Pages

GitHub Pages is an incredible, free resource. It handles TLS certificates, global CDN caching, and high availability completely out of the box. 

And that is exactly the problem. **It abstracts away all the infrastructure.**

If I want to prove my skills in Cloud provisioning, networking, and container orchestration, I cannot rely on a managed static hosting service. I need to be the one managing the servers, configuring the load balancers, and securing the network. 

## The Blueprint for Phase 2

Starting tomorrow, I am drafting the roadmap for Phase 2: Escaping GitHub Pages. 

The goal is to take this modular repository we have structured and deploy it into a production-grade cloud environment. Here is a high-level look at the new stack:

1.  **Containerization:** The frontend shell and the React Admin CMS will be containerized using Docker. This ensures they can run consistently anywhere, completely independent of my local machine.
2.  **Amazon Web Services (AWS):** We will migrate the hosting from GitHub's servers to our own infrastructure on AWS, utilizing EC2 instances for compute power.
3.  **Kubernetes (EKS):** Instead of just running standard Docker containers on a single VM, we will orchestrate them using Amazon Elastic Kubernetes Service (EKS) to ensure high availability and self-healing.
4.  **Infrastructure as Code (IaC):** Most importantly, I will not be clicking around the AWS Console to build this. Every VPC, subnet, security group, and cluster will be provisioned automatically using **Terraform**.

![Phase 2 Architecture Diagram](/portfolio/images/blog16A.png)
*(Caption: A high-level blueprint of the upcoming migration from GitHub Pages to AWS EKS.)*

Phase 1 proved I can build a clever, scrappy solution. Phase 2 will prove I can engineer enterprise-grade infrastructure. 

In [Blog 17](#/blog/2026-09-04-portfolio-blog-17-escaping-github-pages-the-case-for-aws), I will break down the exact limitations of our current setup and the detailed reasoning behind choosing AWS and EKS for the migration.