# Initial AWS Service Mapping

## 1. Purpose

The selected architecture is:

Global Distribution with a Single Primary Application Region
and a Secondary Disaster-Recovery Strategy.

This document maps the conceptual architecture to suitable AWS services.

The service selections are initial architectural decisions and will be
evaluated further based on requirements, alternatives, trade-offs, security,
cost, and operational complexity.

---

## 2. High-Level AWS Architecture

Global Users
     |
     v
Amazon Route 53
     |
     v
Amazon CloudFront
     |
     +-----------------------------+
     |                             |
     v                             v
Amazon S3                    AWS WAF
Static / Media                    |
                                   v
                          Application Load Balancer
                                   |
                                   v
                         EC2 Auto Scaling Group
                          across multiple AZs
                                   |
                                   v
                         Amazon Aurora / RDS
                                   |
                                   v
                             Data Storage

Additional Services:

- AWS IAM
- AWS KMS
- AWS Secrets Manager
- Amazon CloudWatch
- AWS Backup
- Secondary AWS Region for disaster recovery

---

# 3. Amazon Route 53

## Purpose

Amazon Route 53 will provide DNS for the GlobalNews platform.

## Responsibilities

- Resolve the GlobalNews domain name
- Direct users toward the public application entry point
- Support health-aware routing where appropriate
- Support future disaster-recovery traffic routing

## Why It Fits

GlobalNews requires a reliable public DNS service and needs a design that can
later support regional recovery.

## Alternatives

Alternative approaches could include external DNS providers.

## Initial Decision

Amazon Route 53 is preferred because it integrates directly with AWS
infrastructure and supports routing and health-check capabilities.

---

# 4. Amazon CloudFront

## Purpose

Amazon CloudFront will provide the global content-distribution layer.

## Responsibilities

CloudFront can distribute:

- News articles where appropriate
- Images
- Videos
- CSS
- JavaScript
- Other static assets

It can also forward dynamic requests to the application origin.

## Why It Fits

GlobalNews users are distributed across Europe, North America, and Asia.

CloudFront can reduce latency by serving cacheable content closer to users.

It can also reduce the number of requests reaching the application origin
during breaking-news traffic spikes.

## Main Architectural Benefit

Without global caching:

Millions of Users
      |
      v
Application Origin
      |
      v
Very High Origin Load


With global caching:

Millions of Users
      |
      v
CloudFront
      |
      +---- Cached Responses
      |
      v
Only Required Requests
Reach Origin

## Initial Decision

CloudFront is selected as the preferred global distribution service.

---

# 5. Amazon S3

## Purpose

Amazon S3 will provide durable object storage for static and media content.

## Content Examples

- Article images
- Thumbnails
- Videos
- Static website assets
- Media files
- Selected backup data

## Why It Fits

GlobalNews currently stores media files on application servers.

Separating media storage from application compute removes unnecessary storage
and bandwidth pressure from the application servers.

S3 also provides highly scalable object storage.

## Initial Decision

Amazon S3 is selected for static and media object storage.

---

# 6. Application Load Balancer

## Purpose

The Application Load Balancer will distribute dynamic application requests
across multiple healthy application instances.

## Responsibilities

- Distribute HTTP/HTTPS traffic
- Perform health checks
- Route requests only to healthy application resources
- Support application scaling across Availability Zones

## Why It Fits

GlobalNews should not depend on a single application server.

A load balancer allows multiple application instances to process traffic.

## Initial Decision

Application Load Balancer is selected for application traffic distribution.

---

# 7. Amazon EC2 Auto Scaling

## Purpose

EC2 instances will host the dynamic application workload.

An Auto Scaling Group will automatically adjust application capacity according
to demand.

## Responsibilities

- Run application code
- Add capacity during high traffic
- Reduce capacity when traffic falls
- Replace unhealthy instances
- Distribute instances across multiple Availability Zones

## Why It Fits

GlobalNews may experience sudden traffic increases of up to 50 times normal
traffic.

Fixed application capacity would either:

- become overloaded during peak traffic

or

- remain unnecessarily expensive during normal traffic

Auto Scaling allows capacity to respond dynamically.

## Alternative

Container-based compute such as ECS could also be considered.

## Initial Decision

EC2 Auto Scaling is selected as the initial compute approach because it provides
a clear and understandable scaling model for this architecture.

---

# 8. Amazon Aurora / Amazon RDS

## Purpose

The relational database layer will store dynamic application data.

Possible data includes:

- Article metadata
- User information
- Subscription information
- Comments
- Application records

## Requirements

The database should support:

- High availability
- Backups
- Read-heavy workloads
- Scaling where appropriate
- Secure private access
- Disaster recovery

## Alternatives

Possible options include:

- Amazon Aurora
- Amazon RDS
- Amazon DynamoDB

The final relational database selection will be documented through an
Architecture Decision Record.

## Initial Direction

Amazon Aurora or Amazon RDS is currently preferred because GlobalNews contains
relational application data and dynamic application functionality.

---

# 9. AWS IAM

## Purpose

AWS IAM will manage access between users and AWS services.

## Security Principles

The design should follow:

- Least privilege
- Role-based service permissions
- Limited administrative access
- No unnecessary public permissions

## Initial Decision

IAM is required as the central AWS authorization mechanism.

---

# 10. AWS WAF

## Purpose

AWS WAF will help protect public HTTP endpoints.

## Responsibilities

Potential protections include:

- Blocking known malicious request patterns
- Rate-based rules
- Filtering unwanted requests
- Protecting the public application layer

## Why It Fits

GlobalNews is a public media platform that may receive very high traffic.

The architecture should distinguish between legitimate breaking-news demand
and unwanted or malicious traffic.

---

# 11. AWS KMS

## Purpose

AWS KMS can provide encryption-key management for supported AWS services.

## Use Cases

Possible encryption targets include:

- Database storage
- S3 objects
- Backups
- Sensitive application data

---

# 12. AWS Secrets Manager

## Purpose

Secrets Manager can securely store application credentials and secrets.

Examples:

- Database credentials
- Application secrets

Credentials should not be hard-coded into application code or Terraform files.

---

# 13. Amazon CloudWatch

## Purpose

CloudWatch will provide monitoring and operational visibility.

## Monitoring Areas

- Application errors
- CPU and resource utilization
- Application latency
- Load-balancer metrics
- Auto Scaling activity
- Infrastructure health
- Traffic behavior
- Logs

## Why It Fits

GlobalNews requires visibility into both normal traffic and breaking-news
events.

---

# 14. AWS Backup

## Purpose

AWS Backup can help centralize backup policies for supported AWS resources.

## Responsibilities

- Backup scheduling
- Retention management
- Recovery support
- Backup-policy management

The detailed backup strategy will be defined during the disaster-recovery
design.

---

# 15. Secondary AWS Region

## Purpose

A second AWS Region will be used as part of the disaster-recovery strategy.

It will not initially operate as a complete active-active application region.

Possible DR capabilities include:

- Cross-region backup copies
- Replicated media
- Database recovery capability
- Terraform-based infrastructure recreation
- DNS failover

## Why This Approach

This provides protection against primary-region failure without requiring the
cost and complexity of operating the complete platform actively in multiple
regions.

---

# 16. Initial AWS Service Map

| Architecture Area | Proposed AWS Service |
|---|---|
| DNS | Amazon Route 53 |
| Global Content Delivery | Amazon CloudFront |
| Static / Media Storage | Amazon S3 |
| Web Security | AWS WAF |
| Load Balancing | Application Load Balancer |
| Compute | Amazon EC2 |
| Automatic Scaling | EC2 Auto Scaling |
| Database | Amazon Aurora / Amazon RDS |
| Identity & Permissions | AWS IAM |
| Encryption Keys | AWS KMS |
| Secrets | AWS Secrets Manager |
| Monitoring | Amazon CloudWatch |
| Backup | AWS Backup |
| Disaster Recovery | Secondary AWS Region |

---

# 17. Important Architectural Principle

AWS services are not selected simply because they are available.

Every selected service must solve a specific requirement.

For example:

Business Problem:
Global users experience high latency.

        ↓

Technical Requirement:
Content should be delivered closer to users.

        ↓

Architecture Decision:
Use global content distribution and caching.

        ↓

AWS Service:
Amazon CloudFront


Another example:

Business Problem:
Media files overload application servers.

        ↓

Technical Requirement:
Separate media storage from application compute.

        ↓

Architecture Decision:
Use scalable object storage.

        ↓

AWS Service:
Amazon S3