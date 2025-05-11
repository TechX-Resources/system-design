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

## Networking

Networks reliably carry data around the globe, delivering content and applications with high availability. Cloud networking includes:

- Network architecture
- Network connectivity
- Application delivery
- Global performance & delivery
- DNS

### Amazon Route 53 (DNS)

The Domain Name System (DNS) is the Internet’s “phonebook,” translating human‑friendly names (e.g. `www.example.com`) into IP addresses.  
[Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) is a highly available, scalable DNS service with global anycast servers.

**Features:**

- Scales automatically to handle DNS query spikes
- Domain registration & management
- Traffic routing (latency‑, geolocation‑, weighted‑based)
- Health checks & failover

**Tips:**

- Found under **Networking & Content Delivery** in the console
- Route based on users’ geographic location
- Use alias records to map directly to AWS resources

---

## Elasticity in the Cloud

Automatically scale resources up or down based on load—no capacity guessing.

### EC2 Auto Scaling

Monitors your EC2 fleet and adjusts instance count to maintain availability and performance.  
Learn more: [EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

**Features:**

- Scale in/out based on CloudWatch metrics or schedules
- Integrated with EC2, ELB, Spot Fleets, and Predictive Scaling
- Works with SNS for launch/terminate notifications

**Tips:**

- Found on the EC2 Dashboard
- Use predictive scaling to remove manual adjustments

#### AWS Auto Scaling Service

Central console to manage scaling policies across EC2, ECS, DynamoDB, Aurora, and more.

---

## Elastic Load Balancing

Distributes incoming traffic across targets to improve redundancy and performance.

**Tips:**

- Found on the EC2 Dashboard
- Works with EC2, ECS, IP addresses, Lambda functions

AWS offers three types of load balancers:

### Application Load Balancer (ALB)

Layer 7 load balancer for HTTP/HTTPS traffic, ideal for microservices & container-based apps.  
Learn more: [ALB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)

1. Configure Load Balancer
2. Configure Security Settings
3. Configure Security Groups
4. Configure Routing
5. Register Targets
6. Review & Create

### Network Load Balancer (NLB)

Layer 4 load balancer for ultra‑low latency and millions of requests per second.  
Learn more: [NLB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

1. Configure Load Balancer
2. Configure Security Settings
3. Configure Routing
4. Register Targets
5. Review & Create

### Classic Load Balancer (CLB)

Legacy load balancer for EC2‑Classic networks.  
Learn more: [CLB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html)

1. Define Load Balancer
2. Assign Security Groups
3. Configure Security Settings
4. Configure Health Check
5. Add EC2 Instances
6. Add Tags
7. Review & Create

---

## Messaging & Eventing

### Amazon SNS (Simple Notification Service)

Publish/subscribe messaging for push notifications, emails, SMS, and HTTP/S endpoints.  
Learn more: [SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

**Features:**

- Pub/Sub model with topics & subscriptions
- Fan‑out to SQS, Lambda, HTTP/S

**Tips:**

- Found under **Application Integration**
- Topic names ≤ 256 characters

---

### Amazon EventBridge

Serverless event bus to build event‑driven architectures.  
Learn more: [EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)

**Features:**

- Route events from AWS services, SaaS, and custom apps
- Schema registry & discovery

#### EventBridge Pipes

Connect source and target services with filtering & enrichment.  
**Use cases:** microservices decoupling, real‑time monitoring, SaaS integrations

---

### Amazon SQS (Simple Queue Service)

Fully managed message queuing (standard & FIFO) to decouple microservices.  
Learn more: [SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

**Features:**

- Standard (at‑least‑once delivery, best‑effort ordering)
- FIFO (exactly‑once, ordered delivery)

**Tips:**

- Found under **Application Integration**
- FIFO queues: up to 3,000 msg/sec with batching

---

## Containers & Orchestration

### Containers

OS‑level virtualization: bundles application code, dependencies, and runtime into a portable unit.

**Benefits:**

- Lightweight (MB vs GB for VMs)
- Fast startup (seconds vs minutes)
- Consistent across environments

**Runtimes:**

- [Docker](https://www.docker.com/)
- [CRI‑O](https://cri-o.io/)
- [Containerd](https://containerd.io/)
- [OpenVZ](https://openvz.org/)

### Docker

Build, ship, and run containers locally or in the cloud.  
Use [Docker Compose](https://docs.docker.com/compose/) for multi‑container definitions.

- **Image:** Portable template built from a `Dockerfile`
- **Dockerfile:** Text file with instructions to assemble an image

### Amazon ECS (Elastic Container Service)

Fully managed container orchestration for Docker.  
Learn more: [ECS Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

**Tips:**

- Found under **Compute** in the console
- Supports long‑running services, batch jobs, and scheduled tasks

#### Key ECS Terms

- **Task Definition:** JSON/YAML spec for container requirements (CPU, memory, images)
- **Cluster:** Logical grouping of tasks/instances
- **Container Agent:** Connects EC2 instances to a cluster
- **Container Instance:** EC2 instance registered to an ECS cluster

---

## Logging & Auditing

### AWS CloudTrail

Audit all API calls and user activity in your AWS account.  
Learn more: [CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

**Tips:**

- Found under **Management & Governance**
- Event history retained for 90 days
- Up to 5 trails per region

### Amazon CloudWatch

Monitor metrics, logs, and events; set alarms and automated actions.  
Learn more: [CloudWatch User Guide](https://docs.aws.amazon.com/cloudwatch/index.html)

**Features:**

- Custom metrics & dashboards
- Log collection & retention
- Alarms & notifications

---

## Infrastructure as Code

### AWS CloudFormation

Model and provision your entire AWS infrastructure via JSON/YAML templates.  
Learn more: [CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

**Tips:**

- Found under **Management & Governance**
- Templates support macros, nested stacks, and change sets
- You can still manage individual resources in a stack manually

### Governance, Monitoring & Cost Optimization

1. **Resource Cleanup:** Implement lifecycle rules and AWS Config rules to detect unused assets.
2. **Monitoring:** Collect metrics/logs with CloudWatch and use X‑Ray for tracing.
3. **Cost Optimization:** Right‑size instances, use Spot/Savings Plans, review Cost & Usage Reports, and set up AWS Budgets.

---
