
# AWS Cloud Practitioner — CLF-C02 (Page 2)

---

## Table of Contents

1. [Deployment, Scale & Managing Infrastructure](#1-deployment-scale--managing-infrastructure)
2. [Global Infrastructure](#2-global-infrastructure)
3. [Cloud Integrations](#3-cloud-integrations)
4. [Cloud Monitoring](#4-cloud-monitoring)
5. [VPC & Networking](#5-vpc--networking)
6. [Security & Compliance](#6-security--compliance)
7. [Machine Learning](#7-machine-learning)
8. [Account Management & Billing](#8-account-management--billing)
9. [AWS Support Plans](#9-aws-support-plans)
10. [Other Services](#10-other-services)
11. [AWS Architecting & Ecosystem](#11-aws-architecting--ecosystem)

---

## 1. Deployment, Scale & Managing Infrastructure

### CloudFormation

- **Declarative** way of outlining AWS infrastructure as code (IaC)
- Benefits:
  - Full IaC support
  - Cost — resource creation and termination can be automated
  - Auto-generates a diagram from your template
  - Leverage and reuse existing templates

### AWS Cloud Development Kit (CDK)

- Define cloud infrastructure using a **familiar programming language** (Python, TypeScript, Java, etc.)
- Code is **compiled into a CloudFormation template** and deployed

### AWS Elastic Beanstalk — PaaS

- Standard web app architecture: `ELB → EC2 → DB`
- **Developer-centric view** of deploying an application to AWS
- Developer manages the **code only**; AWS handles deployments, scaling, and infrastructure
- Includes **built-in health monitoring**

> Elastic Beanstalk is a **PaaS** offering.

### AWS CodeDeploy

- **Hybrid service** — works with both on-premises and cloud environments
- Automates application deployments
- Enables upgrading applications from **V1 to V2** with minimal manual steps

### AWS CodeCommit

- Git-based **code repository** service
- Private, secure storage for code before it is deployed to AWS

### AWS CodeBuild

- **Serverless** build service in the cloud
- Compiles source code, runs tests, and produces a deployable package
- **Pay only for the build time used**

### AWS CodePipeline

- Connects CodeCommit and CodeBuild into a full **CI/CD pipeline**
- Flow: `Code → Build → Test → Deploy`

### AWS CodeArtifact

- **Artifact management** — storing and retrieving software dependencies
- Developers and CodeBuild can pull dependencies directly from CodeArtifact

### AWS SSM — Systems Manager

- Manage a **fleet of EC2 instances** across both on-premises and cloud
- Hybrid service
- Supports **automated patching**
- Requires the **SSM Agent** to be installed on EC2 instances

### AWS SSM Parameter Store

- **Serverless** secure storage for configuration values and secrets
- Supports version tracking and encryption

---

## 2. Global Infrastructure

> Deploying applications across multiple regions or Edge Locations reduces latency, improves disaster recovery, and increases attack protection.

### Amazon Route 53

- Managed **DNS (Domain Name System)** service
- Routes clients to the correct destination by resolving domain names to IP addresses

#### Routing Policies

| Policy | Health Checks | Behaviour |
|---|---|---|
| **Simple** | No | Direct DNS resolution to a single endpoint |
| **Weighted** | Yes | Distributes traffic across endpoints by assigned weight |
| **Latency** | Yes | Routes users to the nearest / fastest server by location |
| **Failover** | Yes | Routes to primary; fails over to secondary if primary is unhealthy |

### CloudFront

- AWS **CDN (Content Delivery Network)**
- Caches content at **Points of Presence / Edge Locations** worldwide
- Provides **DDoS protection**
- On first request, the Edge Location fetches from the origin (e.g. S3) and caches locally for subsequent requests

### S3 Transfer Accelerator

- Speeds up uploads and downloads to S3 buckets across regions
- Files upload to the nearest **Edge Location** first, then transfer to the target bucket over AWS's **private internal network**

### AWS Global Accelerator

- Optimises routing using the **AWS internal network**
- Applications are accessed via **static IPs**
- Traffic enters the nearest Edge Location and travels internally to the destination

### AWS Outposts

- **Hybrid cloud** solution
- Physical **server racks** installed on-premises that run the same AWS infrastructure and services
- AWS sets up and manages the hardware
- Customer is responsible for **physical security** of the racks

### AWS Wavelength

- Deploys AWS services at the **edge of 5G networks**
- Provides **ultra-low latency** for 5G-connected applications

### AWS Local Zones

- **Extends a VPC and AWS compute/storage/databases** closer to end users
- Acts as an extension of an AWS Region into a specific metropolitan area
- Enables lower latency by hosting EC2 (and other services) in Local Zones

---

## 3. Cloud Integrations

> Applications communicate in two patterns:
> 1. **Synchronous** — Application to Application (direct)
> 2. **Asynchronous / Event-based** — Application → Queue → Application

### AWS SQS — Simple Queue Service

- Producers send messages to a **queue**; consumers poll and process them
- Processed messages are **deleted from the queue**
- **Serverless**
- Used to **decouple applications**
- Default message retention: **4 to 14 days**
- Ordering: **FIFO (First In, First Out)**

### AWS Kinesis Data Streams

- Collects and analyses **real-time streaming data**

### AWS SNS — Simple Notification Service

- Sends a **single message to thousands of subscribers**
- **Pub/Sub** pattern
- Publishers send to a **single SNS Topic**; all subscribers to that topic receive the message

### Amazon MQ

- SQS and SNS are cloud-native; on-premises systems often use **MQTT** or other open protocols
- When migrating to the cloud, **Amazon MQ** provides compatibility with existing on-premises message broker protocols (MQTT, AMQP, etc.)

---

## 4. Cloud Monitoring

### CloudWatch Metrics

- Variables to monitor across AWS services
- Every metric has **timestamps**
- Common metrics: CPU utilisation, status checks, network I/O

### CloudWatch Alarms

- Trigger actions based on any metric threshold
- Alarm actions:
  - Auto Scaling
  - EC2 instance actions
  - SNS notifications
- Supports **billing alarms**

### CloudWatch Logs

- Centralised log collection and storage
- Sources: EBS, ECS, Route 53, and more
- Supports configurable **log retention policies**

> **EC2 CloudWatch Logs**: Logs are not sent by default — you must install the **CloudWatch Logs Agent** and configure what to send.

### EventBridge

- Schedule **Cron jobs** and react to events within AWS
- Example use case: Trigger a security alert when the **root user** signs in

### AWS CloudTrail

- Provides **governance, compliance, and audit** for your AWS account
- Every action taken in the account is logged
- Logs can be sent to **S3** or **CloudWatch Logs**

### AWS X-Ray

- Distributed **tracing** for applications in production
- Provides a **visual representation** of service interactions and request flows
- Used to:
  - Troubleshoot performance issues
  - Pinpoint failing services
  - Identify errors and their causes
  - Determine which users are impacted

### AWS Health Dashboard

| Dashboard | Purpose |
|---|---|
| **Service History** | Health of AWS services across all regions |
| **Your Account** | Personalised alerts and remediation for events affecting your account, including scheduled maintenance |

---

## 5. VPC & Networking

### IP Addresses in AWS

| Type | Description |
|---|---|
| **Public IPv4** | Changes every time an instance is stopped and started |
| **Private IPv4** | Fixed; not accessible from the internet; persists across stop/start |
| **Elastic IP (EIP)** | Fixed public IPv4 address permanently attached to an EC2 instance |
| **IPv6** | Free on AWS; near-unlimited address space (3.4 × 10³⁸) |

> EIP and IPv4 are charged at **$0.005/hour**. IPv6 is free.

### VPC — Virtual Private Cloud

- Linked to a **region**
- **Subnets** are part of a VPC and are associated with a specific **AZ**

| Subnet Type | Accessibility |
|---|---|
| **Public Subnet** | Accessible from the internet |
| **Private Subnet** | Not accessible from the internet |

### Internet Gateway

- Enables a **VPC** to connect to the internet

### NAT Gateway

- Allows **instances in private subnets** to initiate outbound internet connections
- Converts private IP addresses to public IPs (Network Address Translation)

### NACL vs Security Groups

| Feature | NACL | Security Group |
|---|---|---|
| Level | VPC / Subnet level | Instance level |
| Rule types | Allow and Deny | Allow only (deny is implicit) |
| Statefulness | **Stateless** | **Stateful** |

### VPC Flow Logs

- Captures network traffic metadata at VPC, Subnet, or ENI level
- Useful for **debugging networking issues**
- Can be sent to **S3** or **CloudWatch Logs**

### VPC Peering

- Connects two VPCs privately using the **AWS network**
- CIDRs must **not overlap**
- **Not transitive** — peering A↔B and B↔C does not allow A↔C

### VPC Endpoints

- Connect to AWS services over a **private network** (no public internet)
- Benefits: better security, lower latency

| Endpoint Type | Supported Services |
|---|---|
| **VPC Endpoint Gateway** | S3 and DynamoDB only |
| **VPC Endpoint Interface** | All other AWS services |

### AWS PrivateLink

- Most **secure and scalable** way to expose a service from one VPC to thousands of other VPCs
- Traffic never traverses the public internet

### Site-to-Site VPN

- Connects an **on-premises data centre** to a VPC over the **public internet** (encrypted)
- Requires:
  - **Customer Gateway** on the on-premises side
  - **Virtual Private Gateway** on the AWS side

### AWS Direct Connect

- A **dedicated physical connection** between on-premises infrastructure and AWS
- More **private, secure, and consistent** than VPN, but more expensive

### AWS Client VPN

- Connects a **user's computer** to a VPC using **OpenVPN**
- Travels over the internet; if the VPC is connected to on-premises, on-premises resources are also reachable

### Transit Gateway

- Connects **thousands of VPCs** through a central hub
- Eliminates the complexity and sprawl of full-mesh VPC peering

---

## 6. Security & Compliance

### AWS Shared Responsibility Model

| AWS Responsible For | You Responsible For |
|---|---|
| Security **of** the cloud | Security **in** the cloud |
| Managed services infrastructure (S3, DynamoDB, etc.) | OS patching and IAM configuration |
| Physical infrastructure and hardware | Encryption of data |

### DDoS Protection

> DDoS (Distributed Denial of Service) — An attacker uses a botnet to flood your service with millions of requests.

| Service | Layer | Cost |
|---|---|---|
| **AWS Shield Standard** | Layer 3 / 4 | Free |
| **AWS Shield Advanced** | Layer 3 / 4 | $3,000/month — 24/7 DDoS response team |
| **AWS WAF** | Layer 7 | Based on usage |

### AWS WAF — Web Application Firewall

- Protects web applications from exploits at **Layer 7**
- Define **Web ACL rules** to block SQL injection, XSS, and other threats

### AWS Network Firewall

- Full protection from **Layer 3 to Layer 7**
- Covers:
  - VPC-to-VPC traffic
  - Outbound and inbound traffic
  - Direct Connect and Site-to-Site VPN connections

### AWS Firewall Manager

- **Centralised management** of security rules across your organisation
- Manages: VPC Security Groups, WAF rules, Shield Advanced, Network Firewall

### Penetration Testing

- Allowed on your own AWS infrastructure to assess security posture
- No prior approval required for supported services

### Encryption

| Type | Description | Examples |
|---|---|---|
| **Encryption at rest** | Data stored on disk is encrypted | S3, EFS, RDS |
| **Encryption in transit** | Data encrypted while moving between systems | TLS, HTTPS, On-prem → AWS |

### AWS KMS — Key Management Service

- AWS **manages the encryption keys** on your behalf
- You define **who can access** the keys

### AWS CloudHSM

- AWS provides the **encryption hardware (HSM)**
- You **manage the encryption keys** yourself
- HSM = Hardware Security Module

### KMS Key Types

| Key Type | Description |
|---|---|
| **Customer Managed Key** | Created and managed by the customer; can be enabled/disabled |
| **AWS Managed Key** | Created by AWS on your behalf; prefixed with `aws/` |
| **AWS Owned Key** | AWS-managed CMKs used across multiple accounts |
| **CloudHSM Key** | Generated from your own CloudHSM hardware |

### AWS Certificate Manager (ACM)

- Manages and deploys **SSL/TLS certificates**
- Provides HTTPS encryption for websites
- Supports both **public and private** TLS certificates

### AWS Secrets Manager

- Stores and manages **secrets** (database passwords, API keys, etc.)
- Supports **automatic rotation** on a configurable schedule
- Secrets are **encrypted using KMS**

### AWS Artifacts

- Portal providing access to **compliance reports and agreements**
- Includes: Artifact Reports, Artifact Agreements (e.g. BAA, NDA)

### Amazon GuardDuty

- **Intelligent threat detection** powered by ML and third-party threat intelligence
- Analyses: CloudTrail events, VPC Flow Logs, DNS logs
- Optional inputs: S3 logs, EBS logs, Lambda network activity

### Amazon Inspector

- Automated **security vulnerability assessments** for:
  - EC2 instances (via SSM)
  - Amazon ECR container images
  - Lambda functions
- Generates a **risk score** for prioritisation
- Integrates with **AWS Security Hub**

### AWS Config

- **Audits and records compliance** of AWS resource configurations over time
- Tracks configuration changes and flags non-compliant resources
- Data can be stored in S3 and aggregated across accounts

### Amazon Macie

- Uses ML to **discover and protect sensitive data** in S3
- Identifies **PII (Personally Identifiable Information)**

### AWS Security Hub

- **Central security dashboard** across your AWS infrastructure
- Aggregates findings from: GuardDuty, Inspector, Macie, WAF, Firewall Manager, Config, AWS Health, and more

### Amazon Detective

- Analyses the **root cause of security findings**
- Uses **graph analysis and ML** to trace attack paths

### Root User Privileges

Actions reserved for the **root user** only:

- Change account name or password
- Close the AWS account
- View tax invoices
- Sell Reserved Instances in the Marketplace
- Change or cancel the AWS Support plan
- Enable MFA on an S3 bucket
- Sign up for GovCloud

### AWS IAM Access Analyzer

- Identifies resources **shared externally** outside your Zone of Trust
- Zone of Trust = your AWS account or AWS Organisation
- Generates findings when access is detected outside the defined zone

---

## 7. Machine Learning

| Service | Purpose |
|---|---|
| **Amazon Rekognition** | Identify objects, text, scenes, faces, and activities in images and videos |
| **Amazon Transcribe** | Convert speech to text; supports automatic **PII redaction** |
| **Amazon Polly** | Convert text to lifelike speech |
| **Amazon Translate** | Language translation |
| **Amazon Lex** | Powers Alexa; provides ASR (Automatic Speech Recognition) and NLU for chatbots |
| **Amazon Connect** | Cloud-based virtual contact centre; integrates with CRM systems |
| **Amazon Comprehend** | NLP — finds insights and relationships in text (e.g. customer email analysis) |
| **Amazon SageMaker** | Fully managed platform for building, training, and deploying ML models |
| **Amazon Kendra** | Intelligent **document search** using ML indexing |
| **Amazon Personalize** | Real-time **personalised recommendations** (as used by Amazon.com) |
| **Amazon Textract** | **Extract text and data** from scanned documents |

---

## 8. Account Management & Billing

### AWS Organizations

- **Global service** for managing multiple AWS accounts
- Features:
  - **Consolidated billing** — single bill across all accounts
  - **Aggregated usage discounts** — volume pricing across accounts
  - **Reserved Instance pooling** across accounts
  - **SCPs (Service Control Policies)** to restrict account privileges
- Structure: **Root OU (Master Account)** → child OUs → member accounts

### Service Control Policies (SCPs)

- Applied at **OU or account level** — act as IAM permission guardrails
- **Cannot be applied to the master account**
- SCPs require an **explicit Allow** — there is no implicit allow

### Consolidated Billing

| Benefit | Detail |
|---|---|
| Combined usage | Aggregate usage across all accounts for volume discounts |
| Single bill | One invoice for the entire organisation |

### AWS Control Tower

- Easily sets up and governs a **secure, compliant multi-account AWS environment**
- Automates account provisioning and policy enforcement
- Detects **policy violations** automatically

### AWS Resource Access Manager (RAM)

- **Share AWS resources** between accounts — within or outside of AWS Organizations
- Example: Share a VPC subnet between two accounts

### AWS Service Catalog

- **Self-service portal** for launching pre-approved AWS resources
- Admins define a **product list**; users can only deploy what is approved

### AWS Pricing Models

| Model | Description |
|---|---|
| **Pay-as-you-go** | Pay for what you use, when you use it |
| **Save when you reserve** | Commit to 1 or 3 years for significant discounts |
| **Pay less by using more** | Volume-based pricing tiers |
| **Pay less as AWS grows** | Costs decrease as AWS passes on infrastructure efficiencies |

### Service-Specific Pricing

#### Lambda
- Pay per **number of invocations**
- Pay per **compute duration** (GB-seconds)

#### S3
- Number and size of objects stored
- Number and type of requests
- Data transfer **out** of the S3 region

#### EBS
- Volume type and provisioned size (GB/month)
- Number of snapshots

#### RDS
- Instance size, engine, and memory class
- Per-hour billing
- Backup storage is free up to the DB size
- I/O requests per month

#### Networking Costs

| Scenario | Cost |
|---|---|
| Inbound to EC2 | Free |
| EC2 ↔ EC2 in the same AZ (private IP) | Free |
| EC2 ↔ EC2 in different AZs (private IP) | $0.01/GB |
| EC2 ↔ EC2 in different AZs (public IP) | $0.02/GB |

### Savings Plans

| Plan | Discount |
|---|---|
| **EC2 Savings Plan** | Up to **72%** vs On-Demand |
| **Compute Savings Plan** | Up to **66%** vs On-Demand; flexible across instance family, region, OS |

Commit to a specific **$/hour spend** for 1 or 3 years.

### AWS Compute Optimizer

- Analyses usage and recommends **optimal AWS resource configurations**
- Goal: reduce costs and improve performance

### Billing & Costing Tools

| Tool | Purpose |
|---|---|
| **AWS Pricing Calculator** | Estimate costs for a planned solution architecture |
| **Billing Dashboard** | View current month-to-date spend and forecasts |
| **Cost Allocation Tags** | Tag resources to track costs at a granular level (AWS-generated or user-defined) |
| **Cost and Usage Reports (CUR)** | Detailed raw billing and usage data; integrates with Athena for custom analysis |
| **Cost Explorer** | Visualise and manage costs over time; hourly/weekly/monthly views; **12-month forecast** |

### Monitoring Costs

| Tool | Detail |
|---|---|
| **Billing Alarms (CloudWatch)** | Stored only in `us-east-1`; triggers on billing thresholds |
| **AWS Budgets** | Set budgets for cost, usage, reservations, or Savings Plans; up to **5 SNS notifications per budget** |
| **AWS Cost Anomaly Detection** | Uses ML to detect unusual billing patterns; sends an **Anomaly Report with root cause** |
| **AWS Service Quotas** | Alerts when approaching a service quota threshold; integrates with CloudWatch alarms |

### AWS Trusted Advisor

- **High-level account assessment** across 6 categories:

| Category | Examples |
|---|---|
| Cost Optimization | Underutilised instances, idle resources |
| Performance | High-utilisation EC2 instances |
| Security | Open S3 buckets, IAM best practices |
| Fault Tolerance | Multi-AZ, backups |
| Service Limits | Nearing quota thresholds |
| Operational Excellence | Service health, config checks |

> Full checks and programmatic API access require **Business or Enterprise Support Plan**.

---

## 9. AWS Support Plans

| Plan | Response Times | Key Features |
|---|---|---|
| **Basic** | N/A | 24/7 customer service, 7 core Trusted Advisor checks, Personal Health Dashboard |
| **Developer** | General guidance < 24 hrs; System impaired < 12 hrs | Business hours email access to Cloud Support Associates |
| **Business** | Production impaired < 4 hrs; Production down < 1 hr | 24/7 phone, web, chat; full Trusted Advisor + API access; unlimited cases and contacts |
| **Enterprise** | Business-critical < 15 min | Access to **TAM (Technical Account Manager)**; business reviews from AWS experts |
| **Unified Operations (Enterprise+)** | < 15 min | TAM, **DSE** (Domain Specialist Engineer), **SBAS** (Senior Billing and Account Specialist), Migration Specialist; AWS Countdown Premium; Customer Incident Response Team |

---

## 10. Other Services

### AWS STS — Security Token Service

- Generates **short-lived, temporary credentials** for accessing AWS resources
- Credentials have a configurable expiration time

### Amazon Cognito

- Provides **identity management** for web and mobile application users

### AWS Directory Services

- Microsoft Active Directory (AD) manages objects (users, computers, permissions) in a Windows environment
- AWS offers:

| Option | Description |
|---|---|
| **AWS Managed Microsoft AD** | Full AD managed by AWS |
| **AD Connector** | Proxy to your on-premises AD |
| **Simple AD** | Lightweight, standalone directory |

### AWS IAM Identity Center

- Successor to **AWS SSO**
- **Single sign-on** across all AWS accounts in an organisation
- Supports third-party identity providers (e.g. Okta) or the built-in IAM Identity Center store

### Amazon WorkSpaces — DaaS

- **Desktop as a Service** — provision Windows or Linux virtual desktops
- Eliminates on-premises VDI management
- Pay-as-you-go (hourly or monthly)

### Amazon AppStream 2.0

- **Desktop application streaming** to a web browser
- No infrastructure to provision; applications run in AWS and are streamed to users

### AWS IoT Core

- Connects **IoT devices** to the AWS cloud
- **Serverless, secure**; uses a Pub/Sub model

### AWS AppSync

- Build **backends for mobile and web applications** using **GraphQL**

### AWS Amplify

- Full-stack toolset for **developing and deploying web and mobile applications**

### AWS Infrastructure Composer

- Visually **design and build serverless applications** on AWS
- Auto-generates **IaC (Infrastructure as Code)**

### AWS Device Farm

- Tests web and mobile apps against **real desktop and mobile devices / browsers**
- Produces reports, logs, and bug details

### AWS Backup

- Fully managed service to **centralise and automate backups** across AWS services
- Define schedules, retention policies, and frequencies

### AWS Disaster Recovery Strategies

| Strategy | Description | Cost |
|---|---|---|
| **Backup & Restore** | Back up to cloud; restore on-premises when disaster occurs | Cheapest |
| **Pilot Light** | Core components (e.g. DB) run in cloud; scale when needed | Low |
| **Warm Standby** | Full app runs in cloud at reduced capacity; scale up during disaster | Medium |
| **Multi-Site / Hot-Site** | Full production-scale app always running in cloud | Most expensive |

### AWS Elastic Disaster Recovery (DRS)

- Quickly recovers physical, virtual, and cloud-based servers into AWS
- Uses **block-level replication** from on-premises to a staging environment in AWS
- During a disaster, the staging environment is promoted to production

### AWS DataSync

- Moves **large amounts of data** from on-premises to AWS over the network
- Supports **scheduled, incremental** synchronisation

### Cloud Migration Strategies — The 7 Rs

| Strategy | Description |
|---|---|
| **Retire** | Decommission — no longer needed |
| **Retain** | Keep on-premises for now — decision pending |
| **Relocate** | Move to a cloud equivalent (e.g. EC2 to a different VPC) |
| **Rehost (Lift & Shift)** | Migrate as-is to the cloud, no changes |
| **Replatform (Lift & Reshape)** | Migrate with minor cloud optimisations |
| **Repurchase (Drop & Shop)** | Replace with a SaaS product |
| **Refactor / Re-architect** | Redesign using cloud-native features (e.g. monolith → microservices) |

### AWS Application Discovery Service

- Scans on-premises servers to collect data for migration planning
- Two modes:

| Mode | Description |
|---|---|
| **Agentless** | Uses a virtual appliance; discovers VMs |
| **Agent-based** | Installed on servers; captures deeper system data |

- Results viewable in **AWS Migration Hub**

### AWS Application Migration Service (MGN)

- **Lift and Shift (Rehost)** migration service
- A replication agent on on-premises servers performs **continuous block-level replication** into a staging environment in AWS
- At cutover, the staging environment is promoted to production

> **Key distinction:**
> - **AWS MGN** — Migrate servers, applications, and data (**keyword: server**)
> - **AWS DMS** — Migrate databases only

### AWS Migration Evaluator

- Builds a **data-driven business case** for migrating from on-premises to AWS
- Takes a snapshot of on-premises state, analyses it, and models the projected AWS footprint and cost

### AWS Migration Hub

- **Central location** to track and orchestrate migration activities
- Aggregates data from Application Discovery Service and MGN

### AWS Fault Injection Simulator (FIS)

- Implements **Chaos Engineering** — deliberately injects faults to observe how applications behave under stress

### AWS Step Functions

- Build **serverless visual workflows** (state machines) to orchestrate services
- Define pass/fail paths and error handling logic
- Used for multi-step automation and error recovery

### AWS Ground Station

- Manage **satellite communications** and process downlinked data
- Downloads satellite data directly into your AWS VPC within seconds

### Amazon Pinpoint

- Scalable **two-way marketing communications** service
- Supports email, SMS, push notifications, and more

---

## 11. AWS Architecting & Ecosystem

### General Guiding Principles

1. Stop guessing capacity — use Auto Scaling
2. Test at production scale
3. Automate architectural experimentation
4. Allow architecture to evolve over time
5. Drive architecture using data
6. Improve through game days and chaos engineering

### Design Principles

1. **Scalability** — design for growth
2. **Disposable resources** — servers should be easily replaceable
3. **Automation** — reduce manual operations
4. **Loose coupling** — reduce dependencies between components
5. **Think in services, not servers**

---

### AWS Well-Architected Framework — Six Pillars

#### 1. Operational Excellence

> The ability to run and deliver business value while continuously improving processes.

- Design principles:
  - Use Infrastructure as Code (IaC)
  - Make frequent, small, reversible changes
  - Refine operations procedures regularly
  - Anticipate failure
  - Learn from operations events and failures

#### 2. Security

> The ability to protect information, systems, and assets.

- Design principles:
  - Implement a strong identity foundation
  - Apply security at all layers
  - Automate security best practices
  - Protect data in transit and at rest
  - Keep people away from data

#### 3. Reliability

> The ability of a system to recover from failures and meet demand.

- Design principles:
  - Test recovery procedures
  - Automatically recover from failures
  - Stop guessing capacity
  - Manage change through automation

#### 4. Performance Efficiency

> The ability to use compute resources efficiently as demand changes.

- Design principles:
  - Go global in minutes
  - Use serverless architectures
  - Experiment more often
  - Apply mechanical sympathy — understand and use the most suitable AWS services

#### 5. Cost Optimization

> The ability to deliver business value at the lowest possible cost.

- Design principles:
  - Adopt a consumption model
  - Measure overall efficiency
  - Stop spending money on undifferentiated data centre operations
  - Analyse and attribute expenditure

#### 6. Sustainability

> Minimise the environmental impact of cloud workloads.

- Design principles:
  - Understand and quantify your impact
  - Establish sustainability goals
  - Maximise utilisation
  - Adopt new, more efficient hardware and software over time

---

### AWS Well-Architected Tool

- Free tool to **review your architecture** against the six pillars
- Identifies risks and provides improvement recommendations

### AWS Customer Carbon Footprint Tool

- Tracks **carbon emissions** generated from your AWS usage

---

### AWS CAF — Cloud Adoption Framework

> Helps organisations build and execute a comprehensive plan for **digital transformation using AWS**.

#### Six CAF Perspectives

| Perspective | Focus |
|---|---|
| **Business** | Ensure cloud investments accelerate digital transformation and deliver business outcomes |
| **People** | Bridge technology and business; culture, organisational change, workforce readiness |
| **Governance** | Orchestrate cloud initiatives; maximise organisational benefits and manage risk |
| **Platform** | Build enterprise-grade, scalable cloud platforms |
| **Security** | Achieve confidentiality, integrity, and availability of data and cloud workloads |
| **Operations** | Ensure cloud services meet defined business needs and SLAs |

#### CAF Transformation Domains

| Domain | Description |
|---|---|
| **Technology** | Move from legacy infrastructure to cloud |
| **Process** | Leverage ML and data analytics to improve operations |
| **Organisation** | Reimagine your operating model |
| **Product** | Create new value propositions and business models |

#### CAF Transformation Phases

| Phase | Goal |
|---|---|
| **Envision** | Identify how cloud transformation creates business value |
| **Align** | Identify capability gaps across the six CAF perspectives |
| **Launch** | Deliver initial production pilots and demonstrate incremental value |
| **Scale** | Expand successful pilots to meet full business requirements |

---

### AWS Right Sizing

- Match instance types and sizes to actual workload requirements
- Review regularly — needs change over time

### AWS Ecosystem Resources

| Resource | Purpose |
|---|---|
| AWS Blogs | Latest announcements and technical deep-dives |
| AWS Developer Forums | Community Q&A and discussion |
| AWS Whitepapers & Guides | Architecture guidance and best practices |
| AWS Knowledge Centre | Frequently asked questions and solutions |

---

### APN — AWS Partner Network

| Partner Type | Role |
|---|---|
| **Technology Partners** | Provide hardware, software, and tooling |
| **Consulting Partners** | Professional services to help design and build |
| **Training Partners** | Deliver AWS training and certification preparation |

- **AWS Competency Program** — Awarded to APN partners demonstrating customer success in a specialised area

---

### AWS Managed Services (AMS)

- A team that provides **ongoing infrastructure operations and management** on your behalf
- Handles monitoring, patching, backup, and incident response

---

### Key Terminology Recap

| Term | Definition |
|---|---|
| **Scalability** | How a system handles an increase in workload |
| **Elasticity** | How a system automatically scales up and down based on demand |
| **Agility** | How quickly new resources can be provisioned and deployed |

### Quick Reference — Common Confusions

| Service | Use Case |
|---|---|
| **DataSync** | Move data to AWS **over the network** |
| **Storage Gateway** | **Hybrid storage** — bridge between on-premises and AWS |
| **Cost Explorer** | Analyse historical costs, forecast spending, get RI/Savings Plan recommendations |
| **AWS Budgets** | Set budgets and receive **alerts** when thresholds are crossed |
| **CUR** | Detailed **raw billing data** for custom analysis (integrates with Athena) |
| **AWS MGN** | Migrate **servers and applications** (keyword: server) |
| **AWS DMS** | Migrate **databases** only |
