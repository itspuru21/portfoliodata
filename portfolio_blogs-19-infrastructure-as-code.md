---
title: "Portfolio Blog 19 Infrastructure as Code - Automating AWS with Terraform"
description: "Eliminating ClickOps by using Terraform to provision the VPC, Subnets, and EKS cluster defined in our AWS blueprint."
date: "2026-09-03"
---

In [Blog 18](#/blog/2026-09-04-portfolio-blog-18-the-aws-blueprint-designing-the-cloud-architecture), we designed a highly secure, scalable AWS architecture featuring a custom VPC, private subnets, and an Amazon EKS cluster. 

The next step is actually building it. But there is a massive difference between *knowing* how to build cloud infrastructure and doing it *correctly*. Logging into the AWS Management Console and manually clicking through menus to create VPCs and spin up EC2 instances is known as "ClickOps."

For a DevOps engineer, ClickOps is a severe anti-pattern. It is error-prone, unversioned, and impossible to replicate quickly. To build this infrastructure, we need **Infrastructure as Code (IaC)**.

## Why Terraform?

I chose HashiCorp Terraform as my IaC tool. Unlike imperative scripts (where you tell the system *how* to do something step-by-step), Terraform is declarative. You simply write code defining the desired end-state (e.g., "I want a VPC with two private subnets"), and Terraform figures out the API calls to make it happen.

To keep the repository as organized as the architecture itself, I avoided writing a single, massive `main.tf` file. Instead, I modularized the infrastructure setup:

*   `vpc.tf`: Defines the network, subnets, NAT gateways, and routing tables.
*   `eks.tf`: Provisions the Kubernetes control plane and the managed node groups (our EC2 worker instances).
*   `variables.tf`: Keeps the configuration dynamic (e.g., easily swapping AWS regions or instance types).

```hcl
# A snippet from eks.tf defining the Managed Node Group
resource "aws_eks_node_group" "portfolio_nodes" {
  cluster_name    = aws_eks_cluster.portfolio_cluster.name
  node_group_name = "portfolio-worker-nodes"
  node_role_arn   = aws_iam_role.worker_role.arn
  subnet_ids      = aws_subnet.private_subnets[*].id

  scaling_config {
    desired_size = 2
    max_size     = 3
    min_size     = 1
  }
}
```

## Securing the State

Terraform keeps track of the resources it creates using a state file (`terraform.tfstate`). Managing this file securely is a core DevOps competency. 

If I kept the state file locally on my laptop, no one else on a hypothetical team could update the infrastructure, and I risked losing the entire AWS mapping if my hard drive crashed. Worse, state files often contain sensitive secrets in plain text.

To handle this, I configured a remote backend. Terraform stores the state securely in an encrypted **AWS S3 bucket**, and uses an **Amazon DynamoDB table** for state locking, preventing two deployments from accidentally running at the exact same time and corrupting the infrastructure.

![Screenshot of a successful terraform apply command](/portfolio/images/blog19A.png)
*(Caption: Executing `terraform apply` to provision the entire AWS network and EKS cluster in minutes.)*

## The Magic of Apply

Running `terraform apply` and watching dozens of AWS resources spin up automatically in exactly the right configuration is deeply satisfying. What would take hours of careful clicking in the AWS console is deployed flawlessly in minutes.

With the EKS cluster now fully operational in our private subnets, Phase 2 is almost complete. The application is running. But in a distributed containerized system, how do you know if things are actually healthy? 

In [Blog 20](#/blog/2026-09-04-portfolio-blog-20-the-missing-piece-monitoring-and-observability)—the final post of this sprint—we will tackle the absolute necessity of Observability and Monitoring in a Kubernetes environment.