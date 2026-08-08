# AWS Cloud Practitioner — CLF-C02

## Study Priority Guide

| Priority | Label    | Description                          |
|----------|----------|--------------------------------------|
| HIGH     | `HIGH`   | Prioritize for exam preparation      |
| MEDIUM   | `MEDIUM` | Important supporting knowledge       |
| LOW      | `LOW`    | Useful context / lower priority      |

## Priority Map

| Topic                              | Priority |
|------------------------------------|----------|
| Cloud Computing Fundamentals       | HIGH     |
| AWS Global Infrastructure          | HIGH     |
| IAM                                | HIGH     |
| Shared Responsibility Model        | HIGH     |
| EC2                                | HIGH     |
| EC2 Purchasing Options             | HIGH     |
| EC2 Storage                        | HIGH     |
| Scalability, Load Balancing & ASG  | HIGH     |
| S3                                 | HIGH     |
| S3 Storage Classes                 | HIGH     |
| Snowball & Storage Gateway         | MEDIUM   |
| Databases                          | HIGH     |
| Containers                         | MEDIUM   |
| Serverless                         | HIGH     |
| GitHub Actions                     | LOW      |

---

## 1. Cloud Computing Fundamentals

### What is a Server Composed of?

1. CPU
2. RAM / Memory
3. Storage — Database
4. Networking — Routers, DNS, etc.

### Network Workflow

- Client sends data through a **Router** using the Internet
- Client and Server each have their own IP addresses
- Router is responsible for routing the data packet to a **Switch**
- Switch sends the data packet to the correct server

### Traditional IT / Data Centre Disadvantages

1. Pay rent for the data centre
2. Power supply and cooling concerns
3. Adding and replacing hardware (maintenance)
4. Scaling is limited

---

### What is Cloud Computing?

> **On-demand delivery** of compute power, database storage, applications, and other IT resources with **pay-as-you-go pricing** and near-instant access.

> **Key principle: We never pay for the infrastructure — we pay for the resources used.**

### Deployment Models

| Model   | Description |
|---------|-------------|
| **Private Cloud** | Private infrastructure, not exposed to the public. Complete control, more security. |
| **Public Cloud** | AWS, GCP, Azure — owned and operated by third-party, delivered over the internet. |
| **Hybrid Cloud** | Private + Public. Some services on-prem, some on cloud. |

### Five Characteristics of Cloud Computing

1. Completely **on-demand**
2. Broad network access
3. **Multi-tenancy and resource pooling** — multiple customers share services with complete privacy
4. **Rapid elasticity and scalability**
5. Measured services

### Six Advantages of Cloud

1. Trade **CAPEX for OPEX** (Capital Expenses → Operational Expenses)
2. Benefit from massive economies of scale
3. Stop guessing capacity
4. Increased speed and agility
5. Stop spending money running data centres
6. Go global in minutes

### Cloud Computing Service Models

| Model | Managed By You | AWS Manages | Example |
|-------|---------------|-------------|---------|
| **IaaS** — Infrastructure as a Service | App, Data, Runtime, Middleware, OS | Virtualisation, Servers, Storage, Networking | EC2 |
| **PaaS** — Platform as a Service | Application, Data | Everything else | Elastic Beanstalk |
| **SaaS** — Software as a Service | Nothing | Everything | Gmail |

**On-premises** — You manage everything: Applications, Data, Runtime, Middleware, OS, Virtualisation, Servers, Storage, Networking.

### Cloud Pricing Model

| Resource  | Billing Basis                                |
|-----------|----------------------------------------------|
| Compute   | Pay for compute time                         |
| Storage   | Pay for data stored                          |
| Network   | Pay only when data transfers **out** of cloud |

---

## 2. AWS Global Infrastructure

### AWS Regions

- Regions have names, e.g. `us-east-1`
- A region is a **cluster of data centres**
- **Most services are region-specific**

#### How to Choose a Region

| Factor | Consideration |
|---|---|
| **Compliance** | Government may require data to remain local |
| **Latency** | Choose a region close to your users |
| **Available Services** | Not all services exist in every region |
| **Pricing** | Varies by region |

### Availability Zones (AZs)

- **Minimum: 3 / Maximum: 6** per region
- Each AZ consists of one or more discrete data centres
- AZs are physically separated to avoid single points of failure

### Points of Presence / Edge Locations

- **400+** locations worldwide
- Used by CDNs (e.g. CloudFront) to deliver content with low latency

---

## 3. IAM — Identity and Access Management

> IAM is a **global service**.

- **Users** — Individual people within an organisation
- **Groups** — Collections of users
  - Groups contain users; a single user can belong to multiple groups
- Permissions are assigned via **policies** to control access to services

> **Principle of Least Privilege** — Never grant more permissions than needed.

### IAM Policy Structure

```json
{
  "Version": "2012-10-17",
  "Id": "optional-policy-id",
  "Statement": [
    {
      "Sid": "statement-id",
      "Effect": "Allow | Deny",
      "Principal": "account/user/role",
      "Action": ["service:Action"],
      "Resource": ["arn:aws:..."]
    }
  ]
}
```

| Field         | Description                              |
|---------------|------------------------------------------|
| `Version`     | Policy language version                  |
| `Id`          | Optional identifier                      |
| `Sid`         | Statement ID                             |
| `Effect`      | `Allow` or `Deny`                        |
| `Principal`   | Account, user, or role the policy applies to |
| `Action`      | List of API calls                        |
| `Resource`    | Resources the action applies to          |

### IAM Policy Inheritance

- Policy attached at **group level** applies to all group members
- **Inline policy** is used when applying a policy to a user not in any group

### IAM Password Policy

Configurable options:
- Minimum number of characters
- Character case requirements
- Non-alphanumeric character requirements
- Allow IAM users to change their own passwords

### Multi-Factor Authentication (MFA)

> MFA = **Password you know** + **Security device you own**

| MFA Type | Example |
|---|---|
| Virtual MFA device | Google Authenticator, Authy |
| Universal 2nd Factor (U2F) | YubiKey |

### How Users Access AWS

| Method | Access Protection |
|---|---|
| AWS Management Console | Password + MFA |
| CLI (Command Line Interface) | Access Keys |
| SDK (Software Development Kit) | Access Keys |

> **CloudShell** is a browser-based terminal alternative to CLI — not available in all regions.

### IAM Roles for Services

- AWS services sometimes need permissions to perform operations on your behalf
- Use **IAM Roles** for this — never hardcode access keys into services

> **Rule of thumb: Never enter Access Key + Secret into an EC2 instance. Use IAM Roles instead.**

### IAM Security Tools

| Tool | Purpose |
|---|---|
| **IAM Credentials Report** | Lists all account users and their credential status |
| **IAM Access Advisor** | Shows service permissions granted to a user and when they were last used |

### IAM Best Practices

- Apply the principle of least privilege
- Assign users to groups; assign permissions to groups
- Enforce a strong password policy
- Require MFA for all users
- Use IAM Roles for AWS services
- Rotate access keys regularly

### Shared Responsibility — IAM

| AWS Manages | You Manage |
|---|---|
| Infrastructure and global service security | Creating users, groups, and roles |
| Config and vulnerability analysis of IAM service | Enabling MFA |
| | Assigning correct permissions |
| | Rotating access keys |

---

## 4. EC2 — Elastic Compute Cloud

EC2 instances can:
1. Rent virtual machines (VMs)
2. Store data on virtual drives (EBS)
3. Distribute load using a load balancer
4. Scale using Auto Scaling Groups

**Operating Systems:** Linux, Windows, macOS

### EC2 Bootstrapping

- **Bootstrapping** = launching commands automatically when an instance starts
- Runs **only once** at instance start
- Used for: installing updates, software, etc.
- Runs as the **root user**

### EC2 Naming Convention

```
m5.2xlarge
│ │ └── Size within class
│ └──── Generation
└────── Instance class
```

### EC2 Instance Types

| Type | Best For | Notes |
|---|---|---|
| **General Purpose** | Web servers, code repositories | Balanced compute, memory, network |
| **Compute Optimized** (`C` family) | Batch processing, gaming, ML | High-performance processors |
| **Memory Optimized** | Large in-memory datasets, BI databases | High-performance relational/non-relational DBs |
| **Storage Optimized** | OLTP, large read/write datasets, NoSQL | High sequential I/O |

### Security Groups

- Control **inbound and outbound traffic** for EC2 instances
- Rules reference IP addresses or other Security Groups
- By default: **all outbound traffic is allowed**, all inbound is blocked

Security groups regulate:
- Port access
- IP range (CIDR) validation
- Inbound and outbound network traffic

> Security groups are locked to a **region / VPC combination** and can be attached to multiple instances.  
> Best practice: maintain a **separate security group for SSH**.

### Classic Ports to Know

| Port | Protocol |
|---|---|
| 22 | SSH / SFTP |
| 21 | FTP |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP (Windows) |

### SSH Access Summary

| Platform | Method |
|---|---|
| Mac / Linux | SSH (`port 22`) |
| Windows ≥ 10 | SSH (`port 22`) |
| Windows < 10 | PuTTY |
| All platforms | EC2 Instance Connect (browser-based) |

---

## 5. EC2 Purchasing Options

| Option | Discount vs On-Demand | Best For |
|---|---|---|
| **On-Demand** | None (highest cost) | Short-term, unpredictable workloads |
| **Reserved Instances** | Up to **72%** | Steady-state workloads; 1 or 3-year commitment |
| **Savings Plans** | Up to **72%** | Flexible usage commitment (e.g. $10/hr for 3 years) |
| **Spot Instances** | Up to **90%** | Fault-tolerant, flexible workloads — instance can be terminated |
| **Dedicated Hosts** | On-Demand or Reserved pricing | Compliance, BYOL (Bring Your Own License) |
| **Dedicated Instances** | — | Hardware dedicated to you; no control over placement |
| **Capacity Reservations** | None | Reserve capacity in a specific AZ; no time commitment |

### Reserved Instances

- Reserve specific instance attributes (region, OS, tenancy)
- Payment options: No upfront / Partial upfront / All upfront
- Scope: Region or Zone
- Can be **bought and sold** in the Reserved Instance Marketplace

### Savings Plans

- Commit to a **usage amount** (e.g. $10/hr) rather than a specific instance type
- Anything above the committed amount is billed at On-Demand rates

### Dedicated Hosts vs Dedicated Instances

| Feature | Dedicated Hosts | Dedicated Instances |
|---|---|---|
| Physical server dedicated | Yes | No (hardware only) |
| Instance placement control | Yes | No |
| BYOL support | Yes | No |

---

## 6. EC2 Storage

### EBS — Elastic Block Storage

- Acts as a **network-attached volume** for EC2 instances
- Data **persists after instance termination**
- Can only be **mounted to a single EC2 instance** at a time
- **Bound to a specific Availability Zone**
  - An EBS in `us-east-1a` cannot be attached to `us-east-1b`
  - Volumes can be moved across AZs using **Snapshots**

#### EBS Snapshots

- A snapshot = a **backup** of an EBS volume
- Can be **copied across regions and AZs**

| Snapshot Feature | Detail |
|---|---|
| **Snapshot Archive** | 75% cheaper; restore takes 24–72 hours |

### AMI — Amazon Machine Image

- Customise an EC2 instance with your own software, config, and OS
- AMIs are built for a **specific region** but can be copied across regions

### EC2 Image Builder

- Automates the creation of VM and container images
- Process: Build EC2 → Customise → Create AMI → Test → Distribute
- Can be run on a **schedule**

### EC2 Instance Store

- **Physically attached** storage (not a network drive)
- Higher I/O performance than EBS
- **Data is lost if the instance is stopped or terminated** — ephemeral storage

### EFS — Elastic File System

- **Shared network file system**
- Can be **mounted to hundreds of EC2 instances simultaneously**
- Works with **Linux EC2 instances only**
- Available **across multiple AZs**
- More expensive than EBS

#### EFS-IA (Infrequent Access)

- **92% cost reduction** compared to standard EFS
- For files not accessed frequently
- Lifecycle policy example: *Move to EFS-IA if not accessed for 60 days*

### EBS vs EFS

| Feature | EBS | EFS |
|---|---|---|
| Instance attachment | Single instance | Hundreds of instances |
| AZ scope | Single AZ | Multi-AZ |
| OS support | Linux and Windows | Linux only |
| Cross-AZ move | Via snapshot | Native |

### Amazon FSx

Managed service for third-party file systems.

| Variant | Use Case |
|---|---|
| **FSx for Windows File Server** | Windows-native; supports SMB protocol and Windows NTFS |
| **FSx for Lustre** | High Performance Computing (HPC) — ML, video processing; integrates with S3 |

### Shared Responsibility — EC2 Storage

| AWS Manages | You Manage |
|---|---|
| Infrastructure | Snapshot and backup procedures |
| Data replication (EFS/EBS) | Data encryption setup |
| Faulty hardware replacement | Data on drives |

---

## 7. Scalability, Load Balancing & Auto Scaling

### Scalability

| Type | Direction | Method | Example |
|---|---|---|---|
| **Vertical Scaling** | Scale Up / Down | Increase instance size | `t2.micro` → `t2.large` |
| **Horizontal Scaling** | Scale Out / In | Increase number of instances | Add more EC2s |

### High Availability

- Running your application in **at least 2 AZs**
- Goes hand-in-hand with horizontal scaling

### Key Terms

| Term | Definition |
|---|---|
| **Scalability** | Accommodate a larger load by scaling up or out |
| **Elasticity** | Auto-scaling based on actual load (scale out / in automatically) |
| **Agility** | Reduce time to provision resources; faster iteration |

### Elastic Load Balancer (ELB)

- Forwards internet traffic to multiple EC2 instances downstream
- Can distribute load **across multiple AZs**
- AWS manages the load balancer infrastructure; you handle configuration

| Load Balancer Type | OSI Layer | Protocol |
|---|---|---|
| **Application Load Balancer (ALB)** | Layer 7 | HTTP / HTTPS |
| **Network Load Balancer (NLB)** | Layer 4 | TCP / UDP |
| **Gateway Load Balancer** | Layer 3 | GENEVE protocol |

### Auto Scaling Groups (ASG)

- Automatically **scale out** (add instances) when load increases
- Automatically **scale in** (remove instances) when load decreases
- Replaces **unhealthy EC2 instances** automatically

### Auto Scaling Strategies

| Strategy | Description |
|---|---|
| **Manual Scaling** | Adjust capacity manually |
| **Dynamic Scaling** | React to real-time metrics (e.g. CloudWatch alarm) |
| **Target Tracking Scaling** | Keep a metric at a target value (e.g. average CPU at 40%) |
| **Scheduled Scaling** | Scale at a specific time (anticipated load) |
| **Predictive Scaling** | Use ML on historical patterns to forecast and scale proactively |

---

## 8. Amazon S3

- Used for **backup, storage, and static hosting**
- Buckets are **regional** but names must be **globally unique**
  - New feature: **Account Regional Namespace** — AWS adds a suffix, enabling same name across regions

### Bucket Naming Conventions

- No uppercase, no underscores
- Must not be an IP address
- Must start with a lowercase letter or number

### S3 Object Keys

> **Key = Full Path** (Prefix + Object name)

- Example: `s3://my-bucket/folder/subfolder/file.txt`
- Maximum single upload size: **5 TB**
- For uploads > 5 GB, use **multi-part upload**

### S3 Security

| Type | Method |
|---|---|
| **User-based** | IAM Policies |
| **Resource-based** | Bucket Policies, Object ACL, Bucket ACL |
| **Encryption** | Server-side (default) or client-side |

#### S3 Bucket Policy Structure

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow | Deny",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### S3 Features

| Feature | Notes |
|---|---|
| **Static Website Hosting** | URL depends on bucket name and region |
| **Versioning** | Enabled at bucket level; tracks file changes; suspending does not delete old versions |
| **IAM Access Analyzer** | Ensures only intended principals can access your S3 buckets |

### S3 Replication

> Versioning **must be enabled** on both source and destination buckets.

| Type | Use Case |
|---|---|
| **CRR** — Cross-Region Replication | Compliance, lower latency for cross-region users |
| **SRR** — Same-Region Replication | Log aggregation, replicating prod to dev/test |

### S3 Encryption

| Method | Description |
|---|---|
| **Server-Side Encryption** | Default — AWS encrypts objects automatically |
| **Client-Side Encryption** | Client encrypts data before uploading |

### Shared Responsibility — S3

| AWS Manages | You Manage |
|---|---|
| Durability and availability | S3 Versioning |
| Infrastructure and internal security | Bucket policies |
| CVE management | Logging and monitoring |
| | Storage class selection |
| | Client-side encryption |

---

## 9. S3 Storage Classes

| Metric | Value |
|---|---|
| **Durability** | 11 nines (99.999999999%) |
| **Availability (Standard)** | 99.99% |

### Storage Class Comparison

| Class | Availability | Use Case | Notes |
|---|---|---|---|
| **S3 Standard** | 99.99% | Frequently accessed data | Low latency, high throughput — big data, gaming, mobile |
| **S3 Standard-IA** | 99.9% | Infrequent access, rapid retrieval | Lower cost than Standard; retrieval fee applies — disaster recovery |
| **S3 One Zone-IA** | 99.5% | Secondary copy of data | Single AZ; data lost if AZ is destroyed |
| **S3 Glacier Instant Retrieval** | — | Archival with millisecond access | Min. storage: **90 days** |
| **S3 Glacier Flexible Retrieval** | — | Archival with flexible retrieval times | Expedited: 1–5 min; Standard: 3–5 hrs; Bulk: 5–12 hrs; Min. **90 days** |
| **S3 Glacier Deep Archive** | — | Long-term archival | Standard: 12 hrs; Bulk: 48 hrs; Min. **180 days** |
| **S3 Intelligent-Tiering** | — | Automatically moves objects between tiers | Monthly auto-tiering fee |
| **S3 Express One Zone** | — | Ultra-high performance, single AZ | Up to **10x faster** than Standard; millisecond latency; stored in directory buckets |

---

## 10. Snowball & Storage Gateway

### AWS Snowball Family

Physical devices for moving large amounts of data in/out of AWS.

| Device | Storage |
|---|---|
| **Snowball Edge Storage Optimized** | 210 TB |
| **Snowball Edge Compute Optimized** | 28 TB |

#### Snowball Pricing

- Charged for: **device usage** + **data transfer out of AWS**
- **Data transfer into AWS is free**

### AWS Storage Gateway

- A **bridge between on-premises storage and AWS cloud**
- Enables hybrid storage so on-premises environments can seamlessly use AWS services

---

## 11. Databases

### Relational Databases

- Tables are linked to each other via foreign keys
- Queried using **SQL**
- Optimised for **OLTP** (Online Transaction Processing)

### NoSQL Databases

- Non-relational, flexible schema
- Data commonly expressed as **JSON**

### RDS — Relational Database Service

AWS-managed relational database. Supported engines:

- PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM Db2
- **Aurora** (AWS proprietary — see below)

AWS manages: automated provisioning, OS patching, monitoring dashboards, backups.

> **You cannot SSH into RDS instances.**

### Aurora

- AWS proprietary database engine
- Supports **PostgreSQL** and **MySQL**
- ~**20% more expensive** than standard RDS
- **Aurora Serverless**: automated auto-scaling, pay-per-second billing

> All Aurora databases within a cluster share the **same storage volume**.

### RDS Deployment Options

| Option | Description |
|---|---|
| **Read Replicas** | Up to **15 read replicas**; writes go to main DB only |
| **Multi-AZ** | Standby Failover DB in another AZ; automatic failover |
| **Multi-Region** | Read replicas across regions; writes still go to main DB only |

### ElastiCache

- **In-memory database** (Redis / Memcached)
- Reduces load on primary databases for read-intensive workloads
- AWS manages availability, OS updates, and patching

### DynamoDB

- **Serverless NoSQL** database
- Highly available with replication across **3 AZs**
- Handles **millions of requests per second** at single-digit millisecond latency

| Feature | Detail |
|---|---|
| **DAX** (DynamoDB Accelerator) | In-memory cache for DynamoDB; **10x performance** improvement |
| **Global Tables** | Multi-region, active-active replication; low-latency reads and writes globally |

### Redshift

- Based on PostgreSQL — purpose-built **data warehouse**
- Optimised for **OLAP** (Online Analytical Processing)
- Data loaded in batches (hourly, not per second)
- Integrates with **Amazon QuickSight** and **Tableau**
- **Redshift Serverless**: no capacity management needed

### Amazon EMR

- **Elastic MapReduce** — big data processing using **Hadoop clusters**
- Auto-scaling
- Use cases: Data processing, ML, big data analytics

### Amazon Athena

- **Serverless** query service for analysing data directly in **S3**
- Uses standard SQL
- Pricing: **$5 per TB** scanned

### Amazon QuickSight

- Serverless, ML-powered **interactive dashboard** and BI service
- Auto-scalable
- Connects to Aurora, Redshift, and other data sources

### DocumentDB

- AWS-managed **MongoDB-compatible** database
- NoSQL; stores JSON data
- Replicated across **3 AZs**

### Amazon Neptune

- Fully managed **graph database**
- Optimised for highly connected datasets (social networks, knowledge graphs)
- Replicated across **3 AZs**

### Amazon Timestream

- **Serverless time-series** database
- Designed for data that changes over time (IoT, metrics, telemetry)

### AWS Glue

- Managed **ETL (Extract, Transform, Load)** service
- Prepares and transforms data for analytics

### DMS — Database Migration Service

- Migrates data from a **source database to a target database**
- Source database **remains available** during migration

---

## 12. Containers

### Docker

- Software platform for deploying applications in **containers**
- Containers are portable, consistent environments
- Scaling can be done in seconds

### ECS — Elastic Container Service

- Launches Docker containers on AWS
- You manage the underlying EC2 infrastructure
- AWS handles **container scheduling and scaling**
- ECS places containers onto EC2 instances

### Fargate

- Launches Docker containers on AWS — **serverless**
- No infrastructure to provision or manage
- AWS runs containers based on CPU/memory requirements

### ECR — Elastic Container Registry

- Private **Docker image registry** on AWS
- Images stored here can be used by ECS and Fargate

### Amazon EKS — Elastic Kubernetes Service

- Managed **Kubernetes** on AWS
- Containers run inside **EKS Nodes**
- Kubernetes is cloud-agnostic (also runs on GCP, Azure)

---

## 13. Serverless

### AWS Lambda

- **Serverless** compute — run code without managing servers
- Runs **on-demand**, triggered by events
- **Time limit: 15 minutes** per invocation

| Billing Axis | Detail |
|---|---|
| Per request | Number of invocations |
| Per compute time | Duration × memory allocated |

### API Gateway

- Build and expose **serverless HTTPS APIs**
- Allows external clients to invoke Lambda functions
- Supports **RESTful APIs** and **WebSocket APIs**

```
Client → API Gateway → Lambda Function → (Backend / DB)
```

### AWS Batch

- Managed batch processing at any scale
- Runs jobs with a defined **start and end**
- Dynamically provisions **EC2 and Spot Instances** based on job requirements

### AWS Lightsail

- Simplified cloud for users with **little cloud experience**
- Provides VMs, storage, databases, and networking in a single easy interface
- Use cases: simple web applications, blogs, small websites

---

## 14. GitHub Actions

### Workflow Skeleton

```
Workflow
│
├── Event (trigger)
│
└── Jobs
      ├── Job 1
      │     └── Steps
      │           ├── Action
      │           ├── Action
      │           └── Shell Command
      └── Job 2
            └── Steps
```

### GitHub Actions Vocabulary

| Term | Description |
|---|---|
| `name` | Human-readable name for the workflow |
| `on` | Defines the event that triggers the workflow |
| `jobs` | Tasks executed when the workflow runs |
| `runs-on` | Environment for the job (e.g. `ubuntu-latest`, `windows-latest`) |
| `steps` | Ordered sequence of commands or actions within a job |
| `uses` | Execute a reusable action from GitHub or the community |
| `run` | Execute a shell command |
| `with` | Pass inputs to an action |
