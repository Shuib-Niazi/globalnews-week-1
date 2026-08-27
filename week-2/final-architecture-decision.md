# Final Architecture Decision

## 1. Purpose

This document combines the Week 2 architecture evaluations and selects the
architecture that will be developed further for the GlobalNews platform.

The three evaluated options are:

1. Option 1 — Single-Region Architecture
2. Option 2 — Global Distribution with a Single Application Region
3. Option 3 — Multi-Region Application Architecture

The final decision is based on business requirements and architectural
trade-offs rather than selecting the most technically complex solution.

---

## 2. Decision Criteria

The architectures are evaluated against:

- Scalability
- Global performance
- Availability
- Security
- Cost
- Complexity
- Disaster recovery
- Maintainability

Rating scale:

1 = Poor
2 = Weak
3 = Acceptable
4 = Good
5 = Excellent

---

## 3. Decision Matrix

| Criterion | Option 1 | Option 2 | Option 3 |
|---|---:|---:|---:|
| Scalability | 3 | 4 | 5 |
| Global Performance | 2 | 4 | 5 |
| Availability | 3 | 4 | 5 |
| Security | 4 | 5 | 5 |
| Cost Efficiency | 5 | 4 | 2 |
| Low Complexity | 5 | 4 | 2 |
| Disaster Recovery | 2 | 4 | 5 |
| Maintainability | 5 | 4 | 2 |
| **Total** | **29/40** | **33/40** | **31/40** |

---

## 4. Option 1 Evaluation

Option 1 provides the simplest and potentially least expensive architecture.

It can provide good availability within a single region and is relatively easy
to operate.

However, it does not sufficiently address two major GlobalNews challenges:

- Global latency
- Extreme breaking-news traffic

It also remains strongly dependent on one region.

### Decision

**Not selected.**

The simplicity and cost benefits do not sufficiently compensate for the
limitations in global delivery and regional resilience.

---

## 5. Option 3 Evaluation

Option 3 provides the strongest technical architecture in terms of:

- Global performance
- Scalability
- Regional resilience
- Disaster recovery

However, it introduces substantial additional complexity.

A multi-region application requires careful management of:

- Cross-region data replication
- Data consistency
- Traffic routing
- Regional deployments
- Monitoring
- Security
- Failover
- Cross-region networking

It is also expected to have the highest baseline infrastructure and operational
cost.

### Decision

**Not selected as the primary architecture.**

Although technically powerful, the additional complexity and cost are not
currently justified by the GlobalNews requirements.

Multi-region capabilities may be introduced later if business requirements
change.

---

## 6. Option 2 Evaluation

Option 2 combines a globally distributed content-delivery layer with a
centralized primary application region.

This architecture directly addresses the most important GlobalNews problems.

### Global Performance

Frequently requested content can be delivered closer to users across Europe,
North America, and Asia.

### Traffic Spikes

Caching and global distribution can absorb a large percentage of repeated
requests during breaking-news events.

This reduces pressure on the application origin during traffic increases of up
to 50 times normal levels.

### Availability

The primary application can be designed for high availability within its
region.

Globally distributed cached content may also remain available during some
origin problems.

### Security

The global distribution layer provides an opportunity to protect the
application origin and apply security controls before traffic reaches backend
systems.

### Cost

The architecture avoids operating a complete active application stack in
multiple regions while still providing global content-delivery capabilities.

### Maintainability

The main application and data infrastructure remain centralized, reducing the
operational complexity associated with full multi-region deployment.

---

# 7. Selected Architecture

## Option 2 — Global Distribution with a Single Application Region

Option 2 is selected as the target architecture for GlobalNews.

The selection is based on its balance of:

- Global performance
- Scalability
- Availability
- Security
- Cost efficiency
- Operational complexity
- Disaster recovery
- Maintainability

---

## 8. Selected Architecture Concept

Global Users
     |
     v
Global Distribution Layer
     |
     +--------------------------+
     |                          |
     v                          v
Cached / Static Content    Dynamic Requests
                                |
                                v
                      Primary AWS Region
                                |
                         +------+------+
                         |             |
                         v             v
                    Application      Data
                       Layer         Layer
                               
                         Secondary Region
                                |
                                v
                     Disaster Recovery

---

## 9. Important Trade-Off

The selected architecture does not provide active application processing in
multiple regions.

Therefore, dynamic functionality still depends primarily on the selected
application region during normal operation.

This trade-off is accepted because a complete active multi-region application
would introduce significantly greater cost and complexity.

Regional failure will instead be addressed through a dedicated
disaster-recovery strategy.

---

## 10. Future Evolution

If GlobalNews later requires:

- Very low latency for dynamic requests worldwide
- Stronger regional continuity
- Active-active regional processing
- Regulatory geographic separation

the architecture could evolve toward a multi-region model.

The selected design therefore provides a practical architecture for the
current requirements while leaving room for future growth.

---

## 11. Final Decision Statement

GlobalNews will use a globally distributed content-delivery architecture with
one primary highly available application region and a secondary
disaster-recovery strategy.

This architecture provides the best overall balance for the current business
requirements without introducing unnecessary multi-region complexity.