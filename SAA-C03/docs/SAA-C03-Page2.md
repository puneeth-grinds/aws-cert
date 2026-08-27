# 9.0 Solutions Architecture Discussions

This section covers solutions architecture concepts and their design.

## 9.1 Stateless Web Application: WhatIsTheTime.com — Solution Architecture 1

### Requirements

- Allows users to determine the current time
- Does not require a database
- Starts with a small infrastructure
- Can initially accept downtime

### Scenario 1: Initial Architecture

**Architecture:**

- One public EC2 instance
- One Elastic IP address providing a static public IP

**Result:** The application works as expected.

### Scenario 2: Increase in the Number of Users

**Approach: Vertical scaling**

- Increase the EC2 instance size from `t3.micro` to `M5`.

**Result:** Upgrading the instance causes downtime, leaving users dissatisfied.

----
### Scenario 3: Horizontal Scaling

**Approach:**

- Continue using `M5` instances.
- Add more EC2 instances.
- Assign one Elastic IP address to each instance.

**Result:** The additional capacity improves the user experience.

#### Scenario 4: Route 53

**Approach:**

- Configure an **A record** in Route 53.
- Return the IP addresses of the EC2 instances.
- Eliminate the Elastic IP addresses.

**Limitation:** Scaling the EC2 instances remains difficult.

**Result:** The architecture is more optimized.

### Scenario 5: Load Balancing

**Approach:**

- Introduce a public-facing load balancer.
- Protect the EC2 instances using a security group.
- Configure health checks on the load balancer.
- Use an **Alias record** to route traffic from Route 53 to the AWS resource.

**Request flow:**

`Route 53 → Load Balancer → EC2 Instances`

### Scenario 6: Auto Scaling Group

**Approach:**

- Add an Auto Scaling Group to the existing architecture.
- Deploy the architecture within a single Availability Zone.
- Scale in and scale out based on demand.

**Result:** The solution is close to being a good architecture.

### Scenario 7: Disaster Recovery

**Approach:**

- Deploy the architecture across multiple Availability Zones.
- Continue scaling in and out based on demand.

**Result:** The solution is close to being a good architecture and provides improved disaster recovery.

### Scenario 8: Cost Optimization

At least one EC2 instance must run in each Availability Zone. Because these instances are always required, use **Reserved Instances** instead of **On-Demand Instances** to reduce costs.

---

## 9.2 Stateful Web Application: MyClothes.com — Solution Architecture 2

#### Requirements

- Allows users to purchase clothes online
- Provides a shopping cart
- Supports hundreds of users at a time

### Scenario 1: Initial Architecture

**Approach:**

- Reuse the architecture from Solution Architecture 1.

**Problem:**

A user logs in and accesses the application through the first EC2 instance. After adding an item of clothing to the shopping cart, the user attempts to open the cart. However, the request is routed to the second EC2 instance, where the session data is unavailable. As a result, the cart appears empty.

### Scenario 2: Session Stickiness

**Approach:**

- Enable session stickiness so that requests from the same user are routed to the same EC2 instance.

**Result:** This improves the user experience. However, if the EC2 instance is terminated, the session data is still lost.

#### Scenario 3: Cookies

**Approach:**

- Store the shopping-cart contents on the user's device instead of on the EC2 instances.
- Store this information using web cookies.

**Result:** The application becomes stateless. However, cookies can be modified and must be smaller than 4 KB.

### Scenario 4: Server-Side Sessions

**Approach:**

- Create a server-side session.
- Store the session information in ElastiCache using a session ID.
- When the user sends another request, use the session ID to retrieve the corresponding information from ElastiCache.

**Result:** This provides faster performance and improved security.

#### Scenario 5: RDS and RDS Read Replicas

**Approach:**

- Store and retrieve user data using Amazon RDS.
- Use RDS Read Replicas for read-intensive workloads.

#### Scenario 6: Multi-AZ Disaster Recovery

**Approach:**

- Use a Multi-AZ architecture for all applicable components.
- Configure security groups wherever required.

---

### 9.3 Stateful Web Application: MyWordPress.com — Solution Architecture 3

#### Requirements

- Provides a fully scalable WordPress website
- Allows users to access the website and view the correct image updates

#### Scenario 1: Initial Architecture

**Approach:**

- Reuse the architecture from Solution Architecture 1.
- Use Amazon Aurora instead of Amazon RDS.

**Result:** The application provides better scalability.

#### Scenario 2: EBS Volume

**Approach:**

- Use an EBS volume to store images.

**Result:** An EBS volume can be attached to only one EC2 instance at a time.

#### Scenario 3: EFS Instead of EBS

**Approach:**

- Use Amazon EFS to store images.

**Result:** Amazon EFS can be attached to multiple EC2 instances at the same time.

---
