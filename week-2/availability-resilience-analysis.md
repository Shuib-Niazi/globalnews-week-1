# Availability & Failure Resilience Analysis

## 1. Purpose

This analysis evaluates how well each architecture option can continue
operating when infrastructure components fail.

GlobalNews requires high availability and should minimize dependence on a
single infrastructure component or location.

The three architecture options are:

1. Single-Region Architecture
2. Global Distribution with a Single Application Region
3. Multi-Region Application Architecture

---

## 2. Failure Scenarios to Consider

The architecture should consider failures such as:

- One application server fails
- One Availability Zone fails
- The application layer becomes unavailable
- The primary AWS Region becomes unavailable
- A major traffic spike causes infrastructure stress

These failures should not all have the same impact.

---

## 3. Option 1 — Single-Region Architecture

### Availability Assessment

This architecture can still be designed for high availability inside one AWS
Region.

Application resources can be distributed across multiple Availability Zones so
that one server or one Availability Zone does not cause a complete outage.

### Server Failure

If one application server fails, traffic can be directed to healthy application
resources.

Impact:

Low, assuming redundant application capacity exists.

### Availability Zone Failure

If infrastructure is distributed across multiple Availability Zones, the
remaining zone or zones can continue serving traffic.

Impact:

Moderate to Low, depending on capacity and architecture.

### Regional Failure

A complete failure of the primary region creates a major problem.

Because the application and data infrastructure are concentrated in one region,
the system may become unavailable until disaster-recovery procedures are
activated.

### Strengths

- Good protection against individual server failures
- Can provide Multi-AZ high availability
- Relatively simple failure handling
- Easier operations and troubleshooting

### Weaknesses

- Strong dependency on one AWS Region
- Regional failure may cause a major outage
- Disaster recovery must be designed separately

### Availability Rating

**Medium**

---

## 4. Option 2 — Global Distribution with Single Application Region

### Availability Assessment

This architecture adds a globally distributed layer in front of the primary
application region.

This can improve resilience for cacheable content because some content may
continue to be served without every request reaching the origin.

### Server Failure

If one application server fails, other healthy resources in the primary region
can continue processing requests.

Impact:

Low, assuming the application layer is redundant.

### Availability Zone Failure

A Multi-AZ application architecture can continue operating when one
Availability Zone fails.

Impact:

Low to Moderate.

### Application-Origin Failure

Cached content may remain available temporarily even if parts of the
application origin are unavailable.

However, dynamic functionality may be affected.

Examples:

- Login
- Search
- Comments
- Subscriptions
- Personalized content

### Regional Failure

A failure of the primary application region still creates a significant
availability issue for dynamic functionality.

A disaster-recovery strategy is therefore required.

### Strengths

- Better resilience for cached/static content
- Reduces dependency of every request on the origin
- Supports Multi-AZ application availability
- Can continue serving some content during origin problems

### Weaknesses

- Dynamic application functionality still depends on one primary region
- Regional failure remains a major risk
- Recovery strategy is still required

### Availability Rating

**High**

---

## 5. Option 3 — Multi-Region Application Architecture

### Availability Assessment

This architecture provides application infrastructure in multiple AWS Regions.

It therefore offers the strongest protection against regional failure.

### Server Failure

Individual server failure should have very little impact because multiple
application resources are available.

### Availability Zone Failure

Other Availability Zones within the same region can continue serving traffic.

### Regional Failure

If one AWS Region becomes unavailable, traffic can potentially be redirected to
another healthy region.

This provides significantly stronger regional resilience.

### Data Availability

The main challenge is ensuring required data is available in the surviving
region.

The architecture must therefore consider:

- Cross-region replication
- Data consistency
- Failover behavior
- Recovery procedures

### Strengths

- Strongest protection against regional failure
- High availability across multiple geographic locations
- Supports regional failover
- Reduced dependence on one region

### Weaknesses

- Complex data replication
- More complex traffic routing
- Higher operational complexity
- Higher cost
- Failover procedures must be carefully designed and tested

### Availability Rating

**Very High**

---

## 6. Comparison

| Failure Scenario | Option 1 | Option 2 | Option 3 |
|---|---|---|---|
| One server fails | Good | Good | Excellent |
| One AZ fails | Good with Multi-AZ | Good with Multi-AZ | Excellent |
| Application origin failure | Significant impact | Cached content may remain available | Regional alternative may remain available |
| Primary Region fails | Major impact | Major impact on dynamic functionality | Strongest resilience |
| Overall Availability | Medium | High | Very High |

---

## 7. Initial Availability Conclusion

Option 1 can provide good availability for server and Availability Zone
failures, but it remains heavily dependent on one AWS Region.

Option 2 improves resilience because globally distributed cached content can
reduce dependence on the application origin. However, dynamic functionality
still relies on the primary region.

Option 3 provides the strongest availability and regional-failure protection,
but at the cost of significantly greater complexity and expense.

At this stage, Option 2 continues to provide a strong balance between
availability and operational complexity, while Option 3 provides the highest
possible resilience.

The final decision will also depend on security, cost, disaster recovery, and
maintainability.