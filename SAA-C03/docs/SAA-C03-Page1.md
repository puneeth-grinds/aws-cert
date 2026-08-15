# AWS CERTIFIED SOLUTIONS ARCHITECT ASSOCIATE - SAA C03

 ## 1. AWS GLOBAL INFRASTRUCTURE
 - AWS Regions
 - AWS Availability Zones
 - AWS Data Centers
 - AWS Edge Locations/Points of Presence.

 #### 1.1 AWS Regions
 - AWS Regions are **cluster of data centers**
 - Most of the **services are region scoped**

#### 1.2 How do we choose an AWS Region
- Compilance
- Latency
- Available Services
- Pricing

#### 1.3 AWS Availability zones
- Each region has a **minimum of 3 and maximum of 6 AZ's**.
-  Availability Zones are **one or more discrete centers**

#### 1.4 AWS Points of Presence (Edge Locations)
- Amazon has **400+ Edge locations**

#### 1.5 AWS GLOBAL SERVICES
- Identity and Access Management (IAM)
- Route 53 
- CloudFront
- WAF 

> **NOTE**: **AWS Rekognition is a regional service.**

## 2. IAM & AWS CLI
- IAM = Identity and Access Management,  **Global service**
- Users are people in organization, and they can be **grouped**
- **Users can belong to multiple groups, or not belong to any group; both are possible**
> **Effect and Action are mandatory in a IAM Permission**

#### 2.1 IAM Policies Inheritance
- **Inline policies** - Is simply a policy that is embedded directly into one **IAM Identity(User, group or role)** 
- Consits of:
    - Version
    - Id - Optional 
    - Sid - Optional 
    - Statement - required
    - Effect - required
    - Principal - Optional
    - Action - Required
    - Resource - Required
    - Condition - Optional 
