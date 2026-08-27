# Disaster Recovery Analysis

## 1. Purpose

This analysis evaluates how the three proposed architecture options could
recover from major failures and disasters.

Disaster recovery is different from normal high availability.

High availability focuses on keeping the platform operational during failures
such as individual server or Availability Zone failures.

Disaster recovery focuses on restoring service after more serious events such
as a major regional failure, data loss, or prolonged infrastructure outage.

---

## 2. Recovery Objectives

The disaster-recovery strategy must eventually define two important metrics.

### Recovery Point Objective (RPO)

RPO defines how much data loss the business can tolerate.

For example, an RPO of 15 minutes means that, after a disaster, losing up to
15 minutes of recent data may be acceptable.

### Recovery Time Objective (RTO)

RTO defines how long the business can tolerate the platform being unavailable
before service must be restored.

For example, an RTO of 1 hour means that service should be restored within
approximately one hour after a disaster.

Exact RPO and RTO values for GlobalNews have not yet been provided by the
business and must therefore be proposed and justified later.

---

## 3. Important Data Categories

Not all GlobalNews data has the same recovery requirements.

Important categories include:

- Published news articles
- Images
- Videos
- User account information
- Subscription information
- Comments
- Application configuration
- Infrastructure configuration
- Logs and monitoring data

Critical business data should have appropriate backup and recovery protection.

---

## 4. Option 1 — Single-Region Architecture

### Disaster-Recovery Assessment

Option 1 operates primarily within one AWS Region.

Multi-AZ architecture can protect against Availability Zone failures, but it
does not protect the complete platform from a regional disaster.

### Regional Failure

If the primary AWS Region becomes unavailable, the application may become
unavailable until infrastructure and data are restored in another location.

### Required DR Capabilities

A disaster-recovery strategy would need to consider:

- Backups
- Cross-region backup copies
- Infrastructure recreation
- Database restoration
- Media recovery
- DNS or traffic changes
- Recovery procedures

### Advantages

- Simpler normal operating environment
- Simpler backup architecture
- Lower DR complexity during normal operation

### Disadvantages

- Recovery from regional failure may take significant time
- Greater dependence on backups
- Potentially higher RTO
- Potentially greater data-loss exposure depending on backup frequency

### DR Rating

**Medium to Low**

---

## 5. Option 2 — Global Distribution with Single Application Region

### Disaster-Recovery Assessment

Option 2 improves global content availability but the primary dynamic
application still operates in one main region.

The global distribution layer may continue serving some previously cached
public content during an origin problem.

However, dynamic functionality may become unavailable.

Examples include:

- Login
- Search
- Comments
- Subscriptions
- Personalized content

### Regional Failure

A regional disaster would require the application and required data services to
be recovered in another region.

### Required DR Capabilities

The strategy should consider:

- Cross-region backups
- Replication of critical content
- Infrastructure-as-Code templates
- Recovery environment preparation
- DNS or traffic failover
- Database restoration or replication
- Media availability
- Documented recovery procedures

### Advantages

- Some cached public content may remain available during origin disruption
- Global distribution infrastructure reduces dependence on the origin for
  every request
- Main application architecture remains manageable
- Infrastructure as Code can support regional reconstruction

### Disadvantages

- Dynamic functionality still depends on regional recovery
- A secondary recovery environment must be planned
- Data recovery strategy remains essential

### DR Rating

**High with an appropriate recovery design**

---

## 6. Option 3 — Multi-Region Application Architecture

### Disaster-Recovery Assessment

Option 3 provides the strongest foundation for regional disaster recovery
because application infrastructure already exists in multiple regions.

### Regional Failure

If one region becomes unavailable, traffic can potentially be redirected to
another healthy region.

### Data Considerations

Successful regional failover depends heavily on the data architecture.

The design must consider:

- Cross-region replication
- Replication delay
- Data consistency
- Conflict handling
- Backup independence
- Recovery verification

### Advantages

- Fastest potential regional recovery
- Reduced dependence on restoring infrastructure after a regional failure
- Potentially low RTO
- Potentially low RPO when data is continuously replicated
- Strong regional resilience

### Disadvantages

- Highest complexity
- Higher cost
- More difficult data architecture
- Failover procedures must be carefully tested
- Replication errors could affect multiple regions

### DR Rating

**Very High**

---

## 7. Comparison

| DR Criterion | Option 1 | Option 2 | Option 3 |
|---|---|---|---|
| AZ Failure Protection | Good | Good | Excellent |
| Regional Failure Protection | Low | Medium/High with DR | Very High |
| Recovery Complexity | Medium | Medium | High |
| Potential RTO | Higher | Medium | Lower |
| Potential RPO | Backup dependent | Backup/replication dependent | Potentially low |
| Cross-Region Strategy | Mainly backups | Backups + recovery region | Active replication |
| DR Cost | Lower | Medium | High |
| Overall DR Capability | Medium-Low | High | Very High |

---

## 8. Proposed DR Direction

At this stage, exact RPO and RTO values should not be invented because the
business requirements do not specify them.

A reasonable architectural direction for Option 2 would be to maintain the
primary application in one region while protecting critical data and
infrastructure for recovery in a secondary region.

Infrastructure as Code would help recreate required infrastructure
consistently.

Critical data would require an appropriate cross-region backup or replication
strategy.

The exact recovery model will be selected after AWS services are mapped to the
final architecture.

---

## 9. Initial Disaster-Recovery Conclusion

Option 1 provides the weakest protection against complete regional failure.

Option 3 provides the strongest regional continuity but introduces substantial
cost and complexity.

Option 2 can provide a practical middle ground by combining global content
distribution with a documented secondary-region disaster-recovery strategy.

This could provide stronger resilience than Option 1 without requiring the
complete application to operate actively in multiple regions at all times.

The final architecture decision must consider disaster recovery together with
scalability, performance, availability, security, cost, complexity, and
maintainability.