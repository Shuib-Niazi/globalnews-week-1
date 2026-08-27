# ADR-05 — Primary Region and Multi-Region Strategy

## Status

Accepted

## Context

GlobalNews serves users across Europe, North America, and Asia.

The platform requires:

- Good global performance
- High availability
- Support for sudden 50x traffic spikes
- Disaster recovery
- Cost efficiency
- Manageable operational complexity
- Support for future international growth

Two major regional strategies were considered:

1. One primary application region with global content distribution and a
   secondary disaster-recovery strategy
2. Active application infrastructure operating in multiple AWS Regions

A full active multi-region architecture could provide stronger regional
resilience and lower latency for dynamic requests.

However, it would also introduce significantly greater cost and operational
complexity.

## Decision

Use one primary AWS Region for the active application infrastructure.

The primary region will be designed for high availability across multiple
Availability Zones.

Amazon CloudFront will provide global content distribution.

A secondary AWS Region will support disaster recovery rather than operating as
a complete active-active production environment during normal operation.

## High-Level Design

Global Users
     |
     v
Route 53
     |
     v
CloudFront
     |
     +-------------------------+
     |                         |
     v                         v
Cached Content          Dynamic Requests
                              |
                              v
                    PRIMARY AWS REGION
                              |
                   +----------+----------+
                   |                     |
                  AZ-A                  AZ-B
                   |                     |
                   +----------+----------+
                              |
                       Application/Data

                              |
                    Backup / Replication
                              |
                              v
                    SECONDARY AWS REGION
                              |
                              v
                     Disaster Recovery

## Reasons for the Decision

### 1. Global Content Does Not Require a Full Application in Every Region

A large proportion of GlobalNews traffic involves frequently requested
articles, images, videos, and static assets.

CloudFront can distribute this content globally without requiring the complete
application stack to operate in every geographic region.

### 2. Reduced Operational Complexity

A single primary application region avoids the complexity of operating and
coordinating multiple active application environments.

This reduces complexity related to:

- Application deployments
- Database synchronization
- Data consistency
- Cross-region networking
- Monitoring
- Troubleshooting
- Security configuration
- Failover coordination

### 3. Cost Efficiency

An active multi-region architecture would require additional infrastructure to
remain operational across multiple regions.

The selected approach avoids permanently operating duplicate production
capacity when it is not required during normal conditions.

### 4. High Availability Within the Primary Region

Using multiple Availability Zones allows the primary application region to
tolerate failures of individual infrastructure components or an Availability
Zone.

Therefore, normal high availability does not require a second active region.

### 5. Regional Disaster Recovery

A secondary region will provide recovery capability for a major primary-region
failure.

The disaster-recovery design may include:

- Cross-region backups
- Replicated media
- Database recovery or replication
- Terraform configuration
- DNS failover
- Documented recovery procedures

## Alternatives Considered

### Alternative 1 — Single Region Without Secondary Regional Recovery

GlobalNews could operate entirely within one AWS Region and rely only on
Multi-AZ availability.

#### Advantages

- Lowest complexity
- Lower cost
- Easier operations

#### Disadvantages

- Weak protection against complete regional failure
- Potentially long recovery time
- Greater business risk during a regional disaster

This option was rejected because the project requires a disaster-recovery
strategy.

### Alternative 2 — Active-Active Multi-Region

The complete application could operate simultaneously in multiple AWS Regions.

Users could be routed to different active regions.

#### Advantages

- Strong regional resilience
- Potentially lower latency for dynamic requests
- Faster regional failover
- Strong support for international expansion

#### Disadvantages

- Higher cost
- More complex application deployments
- Cross-region database challenges
- Data consistency concerns
- More complex monitoring
- More complex networking
- Greater security-management requirements
- More difficult troubleshooting

This option was not selected because the additional complexity and cost are not
currently justified by the supplied business requirements.

## Trade-Offs

The selected architecture means that dynamic requests normally depend on the
primary application region.

If that region experiences a complete failure, dynamic functionality may be
unavailable until disaster-recovery procedures are activated.

This may result in a higher RTO than an active-active multi-region design.

This trade-off is accepted in exchange for:

- Lower complexity
- Lower baseline cost
- Easier operations
- Simpler data management

## Future Evolution

The architecture can evolve toward active multi-region operation if future
requirements demand:

- Extremely low dynamic-request latency worldwide
- Near-zero regional failover time
- Stronger geographic resilience
- Regulatory data-location requirements
- Significantly larger international workloads

## Consequences

The final production design must define:

- Primary AWS Region
- Multi-AZ architecture
- Secondary disaster-recovery Region
- Cross-region backup strategy
- Media replication strategy
- Database recovery strategy
- DNS failover strategy
- RPO
- RTO
- Disaster-recovery testing procedures
- Terraform strategy for rebuilding infrastructure

## Final Decision Statement

GlobalNews will operate its primary application infrastructure in one highly
available AWS Region across multiple Availability Zones.

Global content will be distributed through CloudFront.

A secondary AWS Region will provide disaster-recovery capability rather than
operating as a full active-active application environment during normal
operation.