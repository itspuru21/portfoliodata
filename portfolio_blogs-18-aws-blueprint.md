---
title: "Portfolio Blog 18 The AWS Blueprint - Designing the Cloud Architecture"
description: "Translating the migration strategy into a concrete architectural blueprint, detailing VPC networking, EKS clusters, and load balancing on AWS."
date: "2026-09-03"
---

In [Blog 17](#/blog/portfolio-blog-17-escaping-github-pages), I established exactly why this portfolio needs to leave the managed "Black Box" of GitHub Pages and migrate to a self-managed AWS environment using containerization. 

Before provisioning a single server, you need a plan. Drawing out the infrastructure ensures you understand the networking, security, and traffic flow required to keep a highly available application running. Here is the concrete AWS blueprint for Phase 2 of this portfolio project.

## The Network Foundation

Everything starts with the network. I won't be deploying resources into the default AWS VPC. To demonstrate proper cloud security practices, I am designing a custom Virtual Private Cloud (VPC) that strictly separates public-facing resources from backend compute.

*   **Public Subnets:** These will host the NAT Gateways and the Application Load Balancer (ALB). They act as the entry point for internet traffic.
*   **Private Subnets:** This is where the actual compute happens. The Amazon EKS worker nodes (EC2 instances) will live here, completely isolated from direct internet access. They can only be reached via the Load Balancer.

By placing the EKS nodes in private subnets, we drastically reduce the attack surface of the application.

![Detailed AWS Architecture Blueprint](/portfolio/images/blog18A.png)
*(Caption: The custom VPC architecture, detailing traffic flow from the internet to the EKS worker nodes.)*

## Compute and Orchestration (EC2 & EKS)

Once the network is secure, we introduce the compute layer. 

As discussed in [Blog 17](#/blog/portfolio-blog-17-escaping-github-pages), we are using Amazon Elastic Kubernetes Service (EKS) to orchestrate our Docker containers. 
*   **The Control Plane:** AWS manages the highly available EKS control plane (the Kubernetes master nodes) across multiple Availability Zones.
*   **The Data Plane (Worker Nodes):** I will provision Managed Node Groups consisting of EC2 instances running in my private subnets. These nodes will pull the Dockerized portfolio image and run it as Kubernetes Pods.

If one EC2 instance crashes, EKS will automatically reschedule the Pods onto a healthy node, ensuring zero downtime.

## Traffic Routing

So, how does a user actually see the website if the containers are locked inside private subnets? 

1.  A user's browser hits the **Application Load Balancer (ALB)** sitting in the public subnet.
2.  The ALB routes the traffic to the EKS cluster.
3.  Inside the cluster, an **AWS Load Balancer Controller** (acting as an Ingress controller) determines exactly which Kubernetes Service and Pod should handle the request.
4.  The response travels back out through the NAT Gateway and the ALB to the user.

This blueprint transforms my portfolio from a simple static site into an enterprise-grade cloud application. But I refuse to build this by clicking through the AWS Console manually. 

In [Blog 19](#/blog/portfolio-blog-19-infrastructure-as-code), I will break down how we take this entire diagram and automate its creation using **Terraform** (Infrastructure as Code).