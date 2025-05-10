# SYSTEM DESIGN

# AWS Compute Guidelines

AWS Compute services are offered on a **pay‑as‑you‑go** basis—you pay only for what you use, with no up‑front commitments (unless you choose reserved options).

## Overview

- **Pricing:** [AWS Pricing](https://aws.amazon.com/pricing/)
- **Free Tiers:**
  - **Free Trials** – Short‑term, full‑feature access
  - **12‑Month Free Tier** – Limited usage for the first year
  - **Always Free** – Monthly usage caps on select services
- **Cost Tracking & Alerts:**
  - **AWS Cost Explorer** (Billing & Cost Management)
  - **AWS Budgets** & **CloudWatch Billing Alarms**

---

## Concurrency & Service Limits

AWS enforces soft limits on concurrent resources (e.g., EC2 instances, Lambda executions, Fargate tasks). These limits:

- **Cover:** EC2, CodeBuild, SageMaker, Lambda, Redshift, Bedrock, Rekognition, Glue, Fargate, EKS, etc.
- **Default values** exceed typical classroom needs but may change without notice.
- **Exceeding limits** may cause resource failures or account restrictions.

**Recommendations:**

1. **Clean up** unused resources regularly.
2. **Request limit increases** via the Support Center if needed.

---

## Core Compute Services

### 1. Amazon EC2 (Elastic Cloud Compute)

Virtual servers (instances) in the cloud—ideal for most workloads.  
Learn more: [EC2 Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-best-practices.html)

#### Pricing Options

- **On‑Demand:** No upfront, pay per second.
- **Reserved Instances:** Commit 1 or 3 years for up to 75% off.
- **Savings Plans:** Flexible, automatic discounts.
- **Spot Instances:** Bid on spare capacity (up to 90% off).
- **Dedicated Hosts/Instances:** Single‑tenant hardware.

#### Key Features

- **Instance Types:** Various CPU, memory, storage, and network profiles.
- **AMIs:** Prebuilt or custom machine images.
- **EBS:** Persistent SSD/HDD volumes, snapshots, and lifecycle management.
- **Networking & Security:** VPC, subnets, security groups, ACLs, Elastic IPs, placement groups.
- **Load Balancing:** ALB, NLB, Classic.
- **Auto Scaling:** Dynamic scaling based on policies and health checks.

#### Best Practices

- Tag resources (`Project`, `Environment`, `Owner`).
- Use IAM roles instead of static credentials.
- Right‑size instances; leverage Spot & Savings Plans.
- Remove unused AMIs, snapshots, and unattached volumes.

---

### 2. Serverless Compute

#### AWS Lambda

- Execute code without servers (15‑min max).
- Triggers: API Gateway, S3, DynamoDB, CloudWatch Events, etc.
- Use **Layers**, minimize cold starts, and monitor with CloudWatch & X‑Ray.
- **Learn more:** [Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

#### AWS Fargate

- Serverless containers for ECS & EKS.
- Pay per vCPU & memory; no EC2 management.

---

### 3. Container Orchestration

#### Amazon ECS

- Managed Docker service with scheduling, service discovery, and load balancing.

#### Amazon EKS

- Managed Kubernetes control plane in your VPC, with IAM and CloudWatch integration.
- **Learn more:** [EKS Workshop](https://www.eksworkshop.com)

---

### 4. Platform & Batch Services

#### AWS Elastic Beanstalk

- PaaS for deploying web apps (Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker).

#### AWS Batch

- Run batch jobs at scale with dynamic resource provisioning.

---

### 5. Other Compute Options

- **Amazon Lightsail:** Simple VPS with predictable pricing.
- **AWS Outposts:** AWS infrastructure on‑premises.
- **AWS App Runner:** Deploy web apps/APIs directly from code or containers.

---

## Governance, Monitoring & Cost Optimization

1. **Resource Cleanup:** AWS Config rules to detect and remove unused assets.
2. **Monitoring:** Collect metrics/logs with CloudWatch; trace with X‑Ray.
3. **Cost Optimization:** Right‑size, use Spot/Savings Plans, and review Cost & Usage Reports—see [AWS Cloud Resource Best Practices](https://support.udacity.com/hc/en-us/articles/4409515588749-AWS-Cloud-Resource-Best-Practices).

---
