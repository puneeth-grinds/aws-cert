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
    - Version - Specifies the policy language version
    - Id - **Optional**; Identifier for the policy 
    - Sid - **Optional**; Optional statementr identifier
    - Statement - **required**; Contains one or more permission statements
    - Effect - **required**; Either `allow` or `deny`
    - Principal - **Optional**; 
    - Action - **Required**; AWS actions allowed or denied
    - Resource - **Required**; AWS resource to which policy is applied
    - Condition - **Optional**; Conditions under which policy is applied
- IAM Permission Example:
![IAM Permission Example](../assets/image.png)

#### 2.2 IAM - Password Policy
- AWS Password policy:
    1. Set a minimum length
    2. Require specific character types\
    3. Allow all IAM users to change their password
    4. Require users to change their password after some time 
    5. Prevent password re-use

#### 2.3 MFA - Multi Factor Authentication
- Used to **protect Root Accounts and IAM Users**
- **MFA = password you know + Security device you own**
- MFA device options:
    1. **Virtual MFA device**: Google authenticator, Authy.
    2. **Universal 2nd Factor (U2F)**: Yubikey and this is **Physical device**