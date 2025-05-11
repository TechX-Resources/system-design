# AWS Guidelines

AWS services operate on a **pay‑as‑you‑go** model—pay only for what you use, with no up‑front commitments (unless you choose reserved options).

---

## Table of Contents

1. [Overview](#overview)
2. [Concurrency & Service Limits](#concurrency--service-limits)
3. [Core Compute Services](#core-compute-services)
4. [Storage & Database Services](#storage--database-services)
5. [Security in the Cloud](#security-in-the-cloud)
6. [Governance, Monitoring & Cost Optimization](#governance-monitoring--cost-optimization)

---

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
- Learn more: [Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

#### AWS Fargate

- Serverless containers for ECS & EKS.
- Pay per vCPU & memory; no EC2 management.

---

### 3. Container Orchestration

#### Amazon ECS

- Managed Docker service with scheduling, service discovery, and load balancing.

#### Amazon EKS

- Managed Kubernetes control plane in your VPC, with IAM and CloudWatch integration.
- Learn more: [EKS Workshop](https://www.eksworkshop.com)

---

### 4. Platform & Batch Services

#### AWS Elastic Beanstalk

- PaaS for deploying web apps (Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker).

#### AWS Batch

- Run batch jobs at scale with dynamic resource provisioning.

---

## Storage & Database Services

### Amazon S3 (Simple Storage Service)

Object storage for virtually unlimited data.

- **Storage Classes:** [Compare classes](https://aws.amazon.com/s3/storage-classes/)
  - Standard, Intelligent‑Tiering, Standard‑IA, One Zone‑IA, Glacier, Glacier Deep Archive
- **Max object size:** 5 TB
- **Features:** Versioning, Lifecycle policies, MFA Delete, Transfer Acceleration

**Tips:**

- Enable **MFA Delete** to protect against accidental losses.
- Use **Transfer Acceleration** for faster transfers over distance.
- Tag buckets for billing and data lifecycle management.

---

### Amazon Glacier & S3 Glacier Deep Archive

Low‑cost, long‑term archival storage.

- Retrieval options: expedited, standard, bulk.
- Ideal for compliance, backups, and infrequently accessed data.

---

### Amazon DynamoDB

Fully managed, serverless NoSQL database.

- **Data models:** Key‑value and document
- **Throughput:** Auto-scaling or provisioned read/write capacity
- **Replication:** Across three AZs; supports global tables
- **Backups:** Point‑in‑time recovery (last 35 days)

**Management Console tabs:**

- **Overview:** Table settings, keys, encryption, ARN
- **Indexes:** Local & global secondary indexes
- **Metrics:** Read/write units, throttling
- **Backups & restores:** On‑demand & continuous
- **Global Tables:** Multi-region replication

**Tips:**

- Use auto-scaling to optimize costs.
- Design keys and indexes for query patterns.
- Leverage DAX (DynamoDB Accelerator) for in-memory caching.

Learn more: [DynamoDB Tutorial](https://aws.amazon.com/tutorials/create-nosql-table/)

---

### Amazon RDS (Relational Database Service)

Managed relational databases with automated administration.

- **Engines:** Aurora, MySQL, MariaDB, PostgreSQL, Oracle, SQL Server
- **Features:** Automated backups, patching, Multi‑AZ high availability, read replicas, encryption

**Console sections:**

- **Connectivity & security:** Endpoints, VPC, security groups
- **Monitoring:** CPU, memory, connections, storage
- **Logs & events:** Audit and error logs
- **Maintenance & backups:** Snapshots, maintenance windows

Learn more: [Aurora Overview](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

---

### Amazon Redshift

Cloud data warehousing for petabyte-scale analytics.

- Columnar storage, MPP architecture.
- Integrates with BI tools, Spectrum for querying S3.
- Encryption in transit & at rest; VPC isolation.

**Tips:**

- Use RA3 nodes for managed storage scaling.
- Leverage workload management (WLM) queues.
- Monitor with CloudWatch and system tables.

---

### Amazon ElastiCache

In-memory caching service for Redis and Memcached.  
Improves application performance by reducing database load.

---

### Amazon Neptune

Managed graph database (Property Graph & RDF).  
Ideal for social networking, fraud detection, and knowledge graphs.

---

### Amazon DocumentDB

MongoDB-compatible, managed document database.  
Designed for JSON workloads and scalable reads.

---

### Amazon CloudFront (CDN)

Global content delivery network.

- Caches content at Edge Locations for low latency.
- Integrates with S3, ALB/ELB, EC2, Lambda@Edge, Shield.
- Features: geo‑restriction, custom TLS, cache-control headers.

**Tips:**

- Use geo‑blocking to restrict content by country.
- Customize cache behaviors per path pattern.
- Invalidate objects or use versioned file names.

---

## Security in the Cloud

Cloud security protects data, applications, and infrastructure. AWS uses a **shared‑responsibility model**: AWS secures the cloud, you secure your workloads.

### AWS WAF & Firewall Manager

Web Application Firewall for HTTP/HTTPS protection.

- **WAF** inspects incoming requests for common exploits.
- **Firewall Manager** centrally manages WAF rules across accounts.

**Tips:**

- Found under **Security, Identity, & Compliance**.
- Protect resources behind CloudFront, ALB, API Gateway.
- Present custom error pages on blocked requests.

Learn more: [AWS WAF & Shield](https://aws.amazon.com/waf/)

### AWS Shield

DDoS protection service.

- **Standard:** Always‑on, free.
- **Advanced:** Paid tier with advanced detection and response.

Learn more: [AWS Shield](https://aws.amazon.com/shield/)

### Identity & Access Management (IAM)

Centralized authentication and authorization for AWS.

- **Users:** Permanent identities with long‑term credentials.
- **Groups:** Collections of users sharing permissions.
- **Roles:** Temporary credentials for AWS services or federated users.
- **Policies:** JSON documents defining allowed/denied actions.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": { "ec2:Region": "us-east-2" }
      }
    }
  ]
}
```

- Validate custom policies with the [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html).
- Learn more: [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

### AWS IAM Identity Center

Federated single sign‑on and centralized account access.

- Manage user access across multiple AWS accounts and applications.
- Provides fine‑grained permission assignments.

Learn more: [IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)

### Amazon Cognito

User authentication, authorization, and synchronization for web & mobile apps.

- **User Pools:** Hosted user directories with sign‑up/in UI
- **Federation:** Social (Google, Facebook) and enterprise (SAML, OIDC)
- **Identity Pools:** Grant access to AWS resources with IAM roles

Learn more: [Amazon Cognito](https://aws.amazon.com/cognito/)

### Governance, Monitoring & Cost Optimization

1. **Resource Cleanup:** Implement lifecycle rules and AWS Config rules to detect unused assets.
2. **Monitoring:** Collect metrics/logs with CloudWatch and use X‑Ray for tracing.
3. **Cost Optimization:** Right‑size instances, use Spot/Savings Plans, review Cost & Usage Reports, and set up AWS Budgets.

---
