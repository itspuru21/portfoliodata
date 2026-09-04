---
title: "Portfolio Blog 20 The Missing Piece - Monitoring and Observability"
description: "Transitioning from aesthetic counters to true observability by implementing monitoring solutions for an EKS environment."
date: "2026-09-03"
---

Over the past few days, we successfully designed an enterprise-grade AWS architecture and provisioned the entire VPC and EKS cluster using Terraform. The infrastructure is live, the containers are orchestrated, and the traffic is routing perfectly through the Application Load Balancer.

But in the cloud, deploying the application is only the beginning. If a pod crashes inside a private subnet, or a node runs out of memory, how do you know? 

The final piece of this architectural puzzle is **Observability**.

## Beyond the Aesthetic Counter

Back in Phase 1, we implemented a simple, database-less visitor counter using a free API. While it served as a neat frontend trick, I explicitly noted its massive architectural weakness: it tracked raw page hits, not actual users. 

In a production AWS environment, that level of monitoring is unacceptable. We need deep visibility into the cluster's CPU usage, memory consumption, network I/O, and application-level errors. 

## The Observability Stack

To achieve this, the standard practice in a Kubernetes ecosystem is deploying a robust monitoring stack:

1.  **Prometheus:** An open-source systems monitoring and alerting toolkit that scrapes metrics directly from the EKS worker nodes and the pods running our frontend and React CMS.
2.  **Grafana:** The visualization layer. It connects to Prometheus to render those raw metrics into beautiful, highly readable dashboards.

![Screenshot of a Grafana monitoring dashboard](/portfolio/images/blog20A.png)
*(Caption: Real-time visibility into the EKS cluster's health and resource utilization.)*

## Operational Intelligence

Deploying these tools isn't just about keeping the lights on; it transforms operations. An active EKS cluster generates a continuous stream of telemetry. Treating this log and metric output as a Big Data streaming source allows for profound insights into system performance. 

This observability stack represents the foundational stage of an operational Business Intelligence life cycle. By aggregating unstructured raw logs and turning them into visual, digestible dashboards, it empowers engineering teams to make structured decisions—like precisely when to scale out node groups or tighten resource limits—rather than guessing based on intuition.

## The End of the Sprint

With monitoring in place, this 20-post sprint comes to a close. 

We started with a fragile, 3-file HTML monolith sitting on my desktop. We built a modular, database-less GitOps architecture powered by a bespoke React CMS. We automated the deployments using GitHub Actions, and finally, we migrated the entire system to a self-healing, Terraform-provisioned Amazon EKS cluster.

This project is no longer just a portfolio. It is a live, breathing demonstration of Cloud and DevOps engineering. Thank you for following along on this journey.