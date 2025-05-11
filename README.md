# AWS Guidelines

AWS services operate on a **pay‑as‑you‑go** model—pay only for what you use, with no up‑front commitments (unless you choose reserved options).

---

## Table of Contents

1. [Overview](#overview)
2. [Getting Started with AWS](#getting-started-with-aws)
3. [Concurrency & Service Limits](#concurrency--service-limits)
4. [Core Compute Services](#core-compute-services)
5. [Storage & Database Services](#storage--database-services)
6. [Networking](#networking)
7. [Security in the Cloud](#security-in-the-cloud)
8. [Elasticity in the Cloud](#elasticity-in-the-cloud)
9. [Messaging & Eventing](#messaging--eventing)
10. [Containers & Orchestration](#containers--orchestration)
11. [Serverless Architecture](#serverless-architecture)
12. [Logging & Auditing](#logging--auditing)
13. [Infrastructure as Code](#infrastructure-as-code)
14. [Governance, Monitoring & Cost Optimization](#governance-monitoring--cost-optimization)
15. [AWS for Students & Education](#aws-for-students--education)
16. [Best Practices & Common Patterns](#best-practices--common-patterns)

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
- **AWS Regions & Availability Zones:**
  - **Regions** - Geographic areas (e.g., us-east-1, eu-west-1)
  - **Availability Zones (AZs)** - Multiple isolated data centers within each region
  - **Edge Locations** - Content delivery network (CDN) endpoints

---

## Getting Started with AWS

### AWS Account Setup

1. **Creating an Account:**

   - Visit [aws.amazon.com](https://aws.amazon.com) and click "Create an AWS Account"
   - Provide email, password, account name, and payment information
   - Complete identity verification
   - Choose a support plan (Free tier recommended for beginners)

2. **Setting Up MFA:**

   - Enable Multi-Factor Authentication for your root account
   - Use authenticator apps like Google Authenticator or Authy

3. **Creating IAM Users:**
   - Create individual IAM users instead of using the root account
   - Assign appropriate permissions through policies or groups

### AWS Management Console

- **Console Layout:**

  - Services menu - Access all AWS services
  - Resource groups - Organize and manage resources
  - Recent history - Track recently used services
  - Favorites - Pin frequently used services

- **First Steps:**
  - Change your password and set up MFA
  - Create IAM users for day-to-day work
  - Create a billing alarm in CloudWatch
  - Explore the AWS Free Tier

### AWS Command Line Interface (CLI)

Install the AWS CLI to interact with AWS services from your terminal:

```bash
# Install AWS CLI (Mac/Linux)
pip install awscli

# Configure credentials
aws configure
```

Enter your AWS Access Key ID, Secret Access Key, default region, and output format when prompted.

---

## Concurrency & Service Limits

AWS enforces soft limits on concurrent resources (e.g., EC2 instances, Lambda executions, Fargate tasks). These limits:

- **Cover:** EC2, CodeBuild, SageMaker, Lambda, Redshift, Bedrock, Rekognition, Glue, Fargate, EKS, etc.
- **Default values** exceed typical classroom needs but may change without notice.
- **Exceeding limits** may cause resource failures or account restrictions.

**Recommendations:**

1. **Clean up** unused resources regularly.
2. **Request limit increases** via the Support Center if needed.
3. **Monitor** your service quotas through the Service Quotas console
4. **Implement** automated cleanup processes for lab environments

**Common Default Limits:**

- EC2: 5 VPCs per region, 5 Elastic IPs per region
- Lambda: 1,000 concurrent executions
- S3: Unlimited buckets (soft limit of 100 per account)
- RDS: 40 instances per region
- CloudFormation: 200 stacks per account

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
  - **General Purpose (T3, M5):** Balanced CPU-to-memory ratio
  - **Compute Optimized (C5):** High CPU-to-memory ratio
  - **Memory Optimized (R5):** High memory-to-CPU ratio
  - **Storage Optimized (I3, D2):** High disk throughput
  - **Accelerated Computing (P3, G4):** GPUs, FPGAs
- **AMIs:** Prebuilt or custom machine images.
- **EBS:** Persistent SSD/HDD volumes, snapshots, and lifecycle management.
- **Networking & Security:** VPC, subnets, security groups, ACLs, Elastic IPs, placement groups.
- **Load Balancing:** ALB, NLB, Classic.
- **Auto Scaling:** Dynamic scaling based on policies and health checks.

#### Instance Launch Process

1. **Choose an AMI** (Amazon Machine Image)
2. **Select an instance type** based on your workload
3. **Configure instance details** (VPC, subnet, IAM role)
4. **Add storage** volumes
5. **Add tags** for organization
6. **Configure security group** (firewall rules)
7. **Review and launch** with key pair for SSH access

#### Best Practices

- Tag resources (Project, Environment, Owner).
- Use IAM roles instead of static credentials.
- Right‑size instances; leverage Spot & Savings Plans.
- Remove unused AMIs, snapshots, and unattached volumes.
- Implement regular backups with automated snapshots
- Use instance metadata service for accessing instance information

---

### 2. Serverless Compute

#### AWS Lambda

- Execute code without servers (15‑min max).
- Triggers: API Gateway, S3, DynamoDB, CloudWatch Events, etc.
- Use **Layers**, minimize cold starts, and monitor with CloudWatch & X‑Ray.
- Learn more: [Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

**Lambda Function Structure:**

```python
def lambda_handler(event, context):
    # Process event data
    print("Received event:", event)

    # Business logic here
    result = process_data(event)

    # Return response
    return {
        'statusCode': 200,
        'body': result
    }
```

**Best Practices:**

- Keep functions focused on a single task
- Reuse the execution context when possible
- Use environment variables for configuration
- Implement proper error handling and retries
- Optimize memory settings for performance and cost

#### AWS Fargate

- Serverless containers for ECS & EKS.
- Pay per vCPU & memory; no EC2 management.
- Suitable for microservices architecture

---

### 3. Container Orchestration

#### Amazon ECS

- Managed Docker service with scheduling, service discovery, and load balancing.
- Components: Task Definitions, Tasks, Services, and Clusters
- Integrates with CloudWatch, IAM, and VPC

#### Amazon EKS

- Managed Kubernetes control plane in your VPC, with IAM and CloudWatch integration.
- Simplifies Kubernetes deployment and management
- Learn more: [EKS Workshop](https://www.eksworkshop.com)

---

### 4. Platform & Batch Services

#### AWS Elastic Beanstalk

- PaaS for deploying web apps (Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker).
- Handles deployment, capacity provisioning, load balancing, and health monitoring
- Supports multiple environments (dev, test, prod)

#### AWS Batch

- Run batch jobs at scale with dynamic resource provisioning.
- Ideal for HPC, big data processing, and ML training jobs
- Integrates with EC2 Spot Instances for cost optimization

---

## Storage & Database Services

### Amazon S3 (Simple Storage Service)

Object storage for virtually unlimited data.

- **Storage Classes:** [Compare classes](https://aws.amazon.com/s3/storage-classes/)
  - Standard, Intelligent‑Tiering, Standard‑IA, One Zone‑IA, Glacier, Glacier Deep Archive
- **Max object size:** 5 TB
- **Features:** Versioning, Lifecycle policies, MFA Delete, Transfer Acceleration, Event Notifications, Access Points

**Common Operations:**

- PUT - Upload objects
- GET - Download objects
- DELETE - Remove objects
- HEAD - Retrieve metadata
- LIST - Enumerate objects

**Security Features:**

- Bucket policies and ACLs
- Block Public Access settings
- S3 Object Lock and Legal Hold
- Server-side encryption (SSE-S3, SSE-KMS, SSE-C)

**Tips:**

- Enable **MFA Delete** to protect against accidental losses.
- Use **Transfer Acceleration** for faster transfers over distance.
- Tag buckets for billing and data lifecycle management.
- Implement appropriate bucket policies and ACLs
- Use S3 Select for efficient querying of data within objects

---

### Amazon Glacier & S3 Glacier Deep Archive

Low‑cost, long‑term archival storage.

- Retrieval options: expedited, standard, bulk.
- Ideal for compliance, backups, and infrequently accessed data.
- Vault Lock policies for WORM (Write Once Read Many) compliance

---

### Amazon DynamoDB

Fully managed, serverless NoSQL database.

- **Data models:** Key‑value and document
- **Throughput:** Auto-scaling or provisioned read/write capacity
- **Replication:** Across three AZs; supports global tables
- **Backups:** Point‑in‑time recovery (last 35 days)

**Key Concepts:**

- Tables, Items (rows), and Attributes (columns)
- Primary keys: Simple (partition key) or Composite (partition key + sort key)
- Secondary indexes: Local (LSI) and Global (GSI)
- Read Consistency: Eventually Consistent (default) or Strongly Consistent

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
- Use TTL (Time to Live) for automatic item expiration
- Consider single-table design for related data

Learn more: [DynamoDB Tutorial](https://aws.amazon.com/tutorials/create-nosql-table/)

---

### Amazon RDS (Relational Database Service)

Managed relational databases with automated administration.

- **Engines:** Aurora, MySQL, MariaDB, PostgreSQL, Oracle, SQL Server
- **Features:** Automated backups, patching, Multi‑AZ high availability, read replicas, encryption

**Management Features:**

- Instance scaling (vertical and horizontal)
- Storage autoscaling
- Automated minor version upgrades
- Performance Insights for query analysis
- Integration with AWS Secrets Manager

**Console sections:**

- **Connectivity & security:** Endpoints, VPC, security groups
- **Monitoring:** CPU, memory, connections, storage
- **Logs & events:** Audit and error logs
- **Maintenance & backups:** Snapshots, maintenance windows

**Best Practices:**

- Use parameter groups to optimize database settings
- Implement proper backup strategies
- Use read replicas for read-heavy workloads
- Enable enhanced monitoring for detailed metrics
- Implement proper security groups and network ACLs

Learn more: [Aurora Overview](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

---

### Amazon Redshift

Cloud data warehousing for petabyte-scale analytics.

- Columnar storage, MPP architecture.
- Integrates with BI tools, Spectrum for querying S3.
- Encryption in transit & at rest; VPC isolation.

**Redshift Node Types:**

- RA3 - Managed storage with high-performance local SSDs
- DC2 - Compute-intensive workloads with local SSD storage
- DS2 - Large data warehouses with HDD storage

**Tips:**

- Use RA3 nodes for managed storage scaling.
- Leverage workload management (WLM) queues.
- Monitor with CloudWatch and system tables.
- Use distribution keys and sort keys for query optimization
- Implement automated snapshot schedules
- Consider Redshift Spectrum for querying data directly in S3

---

### Amazon ElastiCache

In-memory caching service for Redis and Memcached.  
Improves application performance by reducing database load.

**Key Features:**

- Sub-millisecond latency
- High availability with Multi-AZ
- Automatic failover (Redis)
- Redis clustering for sharding
- Backup and restore (Redis)
- Encryption at rest and in transit

**Use Cases:**

- Session storage
- Database caching
- Real-time analytics
- Leaderboards and counters
- Pub/Sub messaging (Redis)

---

### Amazon Neptune

Managed graph database (Property Graph & RDF).  
Ideal for social networking, fraud detection, and knowledge graphs.

**Query Languages:**

- Apache TinkerPop Gremlin
- SPARQL 1.1
- openCypher

---

### Amazon DocumentDB

MongoDB-compatible, managed document database.  
Designed for JSON workloads and scalable reads.

**Key Features:**

- MongoDB 3.6 & 4.0 compatibility
- Automatic scaling up to 64 TB per cluster
- Up to 15 read replicas
- Point-in-time recovery
- Continuous backup

---

### Amazon CloudFront (CDN)

Global content delivery network.

- Caches content at Edge Locations for low latency.
- Integrates with S3, ALB/ELB, EC2, Lambda@Edge, Shield.
- Features: geo‑restriction, custom TLS, cache-control headers.

**Distribution Types:**

- Web Distribution - HTTP/HTTPS content
- RTMP Distribution - Media streaming

**Tips:**

- Use geo‑blocking to restrict content by country.
- Customize cache behaviors per path pattern.
- Invalidate objects or use versioned file names.
- Implement signed URLs or signed cookies for private content
- Use Origin Access Identity (OAI) with S3 origins

---

## Networking

Networks reliably carry data around the globe, delivering content and applications with high availability. Cloud networking includes:

- Network architecture
- Network connectivity
- Application delivery
- Global performance & delivery
- DNS

### Amazon VPC (Virtual Private Cloud)

Logically isolated section of the AWS cloud where you can launch resources in a defined virtual network.

**Key Components:**

- **Subnets:** Segments of IP address ranges in a VPC
  - Public subnets - Direct internet access
  - Private subnets - No direct internet access
- **Route Tables:** Control traffic directions
- **Internet Gateway:** Connects VPC to the internet
- **NAT Gateway:** Allows private subnet resources to access the internet
- **Security Groups:** Instance-level firewall (stateful)
- **Network ACLs:** Subnet-level firewall (stateless)
- **VPC Endpoints:** Private connections to AWS services
- **VPC Peering:** Connect VPCs together
- **Transit Gateway:** Hub for connecting multiple VPCs and on-premises networks

**VPC CIDR Blocks:**

- Primary CIDR must be between /16 (65,536 IPs) and /28 (16 IPs)
- Can add secondary CIDR blocks
- Cannot overlap with other VPCs if peering

**Best Practices:**

- Plan IP addressing carefully
- Use separate subnets for different tiers
- Implement proper security groups and NACLs
- Use VPC Flow Logs for network monitoring
- Implement least privilege access

### Amazon Route 53 (DNS)

The Domain Name System (DNS) is the Internet's "phonebook," translating human‑friendly names (e.g. www.example.com) into IP addresses.  
[Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) is a highly available, scalable DNS service with global anycast servers.

**Features:**

- Scales automatically to handle DNS query spikes
- Domain registration & management
- Traffic routing (latency‑, geolocation‑, weighted‑based)
- Health checks & failover

**Routing Policies:**

- Simple - Map domain to single resource
- Weighted - Distribute traffic across resources
- Latency - Route based on lowest latency
- Failover - Active-passive failover
- Geolocation - Route based on user location
- Geoproximity - Route based on resource location
- Multivalue Answer - Return multiple values

**Tips:**

- Found under **Networking & Content Delivery** in the console
- Route based on users' geographic location
- Use alias records to map directly to AWS resources
- Implement health checks for automatic failover
- Create private hosted zones for internal DNS resolution

### AWS Direct Connect

Dedicated network connection from on-premises to AWS.

**Benefits:**

- Reduced network costs
- Increased bandwidth throughput
- Consistent network experience
- Private connectivity to VPC

**Connection Types:**

- Dedicated Connections (1 Gbps or 10 Gbps)
- Hosted Connections (50 Mbps to 10 Gbps)

### AWS Transit Gateway

Hub-and-spoke network architecture for connecting VPCs and on-premises networks.

**Features:**

- Centralized connectivity management
- Simplified routing
- Global transit network
- Multicast support

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

**Components:**

- Launch Templates/Configurations - Define the EC2 instances
- Auto Scaling Groups - Manage the group of instances
- Scaling Policies - Define when to scale in/out

**Scaling Types:**

- **Dynamic Scaling** - Respond to changing demand
- **Predictive Scaling** - Schedule scaling based on forecast
- **Scheduled Scaling** - Scale based on known traffic patterns

**Tips:**

- Found on the EC2 Dashboard
- Use predictive scaling to remove manual adjustments
- Configure appropriate cooldown periods
- Implement gradual scaling policies
- Use lifecycle hooks for custom actions during scaling events

#### AWS Auto Scaling Service

Central console to manage scaling policies across EC2, ECS, DynamoDB, Aurora, and more.

---

## Elastic Load Balancing

Distributes incoming traffic across targets to improve redundancy and performance.

**Tips:**

- Found on the EC2 Dashboard
- Works with EC2, ECS, IP addresses, Lambda functions
- Implement health checks to ensure traffic routes to healthy targets
- Use sticky sessions for session persistence when needed

AWS offers three types of load balancers:

### Application Load Balancer (ALB)

Layer 7 load balancer for HTTP/HTTPS traffic, ideal for microservices & container-based apps.  
Learn more: [ALB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)

**Features:**

- Path-based routing
- Host-based routing
- Support for WebSockets
- HTTP/2 support
- Integration with AWS WAF
- Authentication through Cognito or OIDC

**Configuration Steps:**

1. Configure Load Balancer
2. Configure Security Settings
3. Configure Security Groups
4. Configure Routing
5. Register Targets
6. Review & Create

### Network Load Balancer (NLB)

Layer 4 load balancer for ultra‑low latency and millions of requests per second.  
Learn more: [NLB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

**Features:**

- Ultra-high performance
- Static IP addresses
- Preserve client IP addresses
- Support for TCP, UDP, and TLS protocols
- Integration with AWS Certificate Manager

**Configuration Steps:**

1. Configure Load Balancer
2. Configure Security Settings
3. Configure Routing
4. Register Targets
5. Review & Create

### Classic Load Balancer (CLB)

Legacy load balancer for EC2‑Classic networks.  
Learn more: [CLB Introduction](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html)

**Features:**

- Basic load balancing for HTTP/HTTPS and TCP
- Sticky sessions using cookies
- Integrates with EC2-Classic

**Configuration Steps:**

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
- Delivery status tracking
- Message filtering
- Message attributes

**Supported Protocols:**

- HTTP/HTTPS
- Email/Email-JSON
- SQS
- Lambda
- SMS
- Mobile Push

**Tips:**

- Found under **Application Integration**
- Topic names ≤ 256 characters
- Implement proper error handling for message delivery
- Use message filtering to reduce unnecessary processing
- Implement dead-letter queues for undeliverable messages

---

### Amazon EventBridge

Serverless event bus to build event‑driven architectures.  
Learn more: [EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)

**Features:**

- Route events from AWS services, SaaS, and custom apps
- Schema registry & discovery
- Event filtering and transformation
- Archive and replay events
- Custom event buses

**Event Structure:**

```json
{
  "version": "0",
  "id": "6a7e8feb-b491-4cf7-a9f1-bf3703467718",
  "detail-type": "EC2 Instance State-change Notification",
  "source": "aws.ec2",
  "account": "111122223333",
  "time": "2017-12-22T18:43:48Z",
  "region": "us-west-1",
  "resources": ["arn:aws:ec2:us-west-1:123456789012:instance/i-1234567890abcdef0"],
  "detail": {
    "instance-id": "i-1234567890abcdef0",
    "state": "terminated"
  }
}
```

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
- Visibility timeout
- Dead-letter queues
- Long polling
- Message retention (up to 14 days)

**Message Lifecycle:**

1. Producer sends message to queue
2. Message is stored redundantly across SQS servers
3. Consumer polls and processes the message
4. Message is deleted by consumer after processing

**Tips:**

- Found under **Application Integration**
- FIFO queues: up to 3,000 msg/sec with batching
- Standard queues: nearly unlimited throughput
- Use appropriate visibility timeout based on processing time
- Implement dead-letter queues for problematic messages
- Use batch operations when possible for better performance

---

## Containers & Orchestration

### Containers

OS‑level virtualization: bundles application code, dependencies, and runtime into a portable unit.

**Benefits:**

- Lightweight (MB vs GB for VMs)
- Fast startup (seconds vs minutes)
- Consistent across environments
- Resource efficiency
- Application isolation

**Runtimes:**

- [Docker](https://www.docker.com/)
- [CRI‑O](https://cri-o.io/)
- [Containerd](https://containerd.io/)
- [OpenVZ](https://openvz.org/)

### Docker

Build, ship, and run containers locally or in the cloud.  
Use [Docker Compose](https://docs.docker.com/compose/) for multi‑container definitions.

- **Image:** Portable template built from a Dockerfile
- **Dockerfile:** Text file with instructions to assemble an image
- **Container:** Running instance of an image
- **Registry:** Repository for storing and distributing images

**Sample Dockerfile:**

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

### Amazon ECS (Elastic Container Service)

Fully managed container orchestration for Docker.  
Learn more: [ECS Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

**Launch Types:**

- EC2 - You manage the infrastructure
- Fargate - Serverless, AWS manages the infrastructure

**Tips:**

- Found under **Compute** in the console
- Supports long‑running services, batch jobs, and scheduled tasks
- Integrates with CloudWatch, IAM, VPC, and Load Balancers
- Use task placement strategies for optimal deployment
- Implement service auto scaling for dynamic workloads

#### Key ECS Terms

- **Task Definition:** JSON/YAML spec for container requirements (CPU, memory, images)
- **Cluster:** Logical grouping of tasks/instances
- **Container Agent:** Connects EC2 instances to a cluster
- **Container Instance:** EC2 instance registered to an ECS cluster
- **Service:** Maintains a specified number of task instances
- **Task:** Instance of a task definition running one or more containers

### Amazon ECR (Elastic Container Registry)

Fully-managed Docker container registry for storing, managing, and deploying container images.

**Features:**

- Private repositories
- Image scanning
- Lifecycle policies
- Cross-region replication
- Integration with ECS and EKS

---

## Serverless Architecture

### AWS Serverless Application Model (SAM)

Open-source framework for building serverless applications on AWS.

**Components:**

- Template specification
- CLI for local testing and deployment
- Integration with AWS services

**Sample SAM Template:**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Resources:
  HelloWorldFunction:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: index.handler
      Runtime: nodejs14.x
      CodeUri: ./src
      Events:
        Api:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

### AWS Step Functions

Visual workflow service for coordinating distributed applications.

**Features:**

- Visual workflow designer
- State machines for application coordination
- Integration with AWS services
- Error handling and retry logic
- Parallel execution

**State Types:**

- Task - Perform an action
- Choice - Add branching logic
- Parallel - Execute branches in parallel
- Map - Process items in an array
- Wait - Delay execution
- Succeed/Fail - End execution

### AWS AppSync

Managed GraphQL service for building real-time applications.

**Features:**

- Real-time data synchronization
- Offline data access
- Built-in security
- Integration with DynamoDB, Lambda, and more
- Schema-first development

---

## Logging & Auditing

### AWS CloudTrail

Audit all API calls and user activity in your AWS account.  
Learn more: [CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

**Types of Events:**

- Management events (control plane operations)
- Data events (data plane operations)
- Insights events (unusual activity)

**Tips:**

- Found under **Management & Governance**
- Event history retained for 90 days
- Up to 5 trails per region
- Enable log file validation for integrity
- Use CloudWatch Logs integration for real-time monitoring
- Encrypt log files with KMS

### Amazon CloudWatch

Monitor metrics, logs, and events; set alarms and automated actions.  
Learn more: [CloudWatch User Guide](https://docs.aws.amazon.com/cloudwatch/index.html)

**Features:**

- Custom metrics & dashboards
- Log collection & retention
- Alarms & notifications
- Synthetic monitoring
- ServiceLens for application insights

**Core Services:**

- CloudWatch Metrics - Time-series data
- CloudWatch Logs - Log collection and analysis
- CloudWatch Events - Event-based triggers
- CloudWatch Alarms - Notifications based on metrics
- CloudWatch Dashboards - Visualization

**Best Practices:**

- Set up appropriate retention periods for logs
- Create custom dashboards for important metrics
- Configure alarms for critical thresholds
- Use metric filters to extract data from logs
- Implement detailed monitoring for critical resources

### AWS X-Ray

Distributed tracing service for analyzing application behavior.

**Features:**

- End-to-end tracing
- Service maps
- Performance bottleneck identification
- Root cause analysis
- Integration with AWS services

---

## Infrastructure as Code

### AWS CloudFormation

Model and provision your entire AWS infrastructure via JSON/YAML templates.  
Learn more: [CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

**Components:**

- Templates - JSON/YAML files defining resources
- Stacks - Collection of resources created from a template
- Change Sets - Preview of changes before implementation

**Sample Template Structure:**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Simple EC2 Instance'

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0c55b159cbfafe1f0

Outputs:
  InstanceId:
    Description: The Instance ID
    Value: !Ref MyEC2Instance
```

**Tips:**

- Found under **Management & Governance**
- Templates support macros, nested stacks, and change sets
- You can still manage individual resources in a stack manually
- Use drift detection to identify manual

### Governance, Monitoring & Cost Optimization

1. **Resource Cleanup:** Implement lifecycle rules and AWS Config rules to detect unused assets.
2. **Monitoring:** Collect metrics/logs with CloudWatch and use X‑Ray for tracing.
3. **Cost Optimization:** Right‑size instances, use Spot/Savings Plans, review Cost & Usage Reports, and set up AWS Budgets.

---
