# Cost Analysis

## 1. Purpose

This analysis compares the cost characteristics of the three proposed
GlobalNews architecture options.

The goal is not simply to choose the cheapest architecture.

The objective is to identify an architecture that provides an appropriate
balance between:

- Performance
- Scalability
- Availability
- Security
- Operational complexity
- Disaster recovery
- Cost

GlobalNews experiences relatively predictable normal traffic but may experience
traffic increases of up to 50 times normal levels during major breaking-news
events.

The architecture should therefore avoid unnecessary permanent capacity while
still being able to respond to sudden demand.

---

## 2. Main Cost Drivers

The main architecture cost areas are expected to include:

- Application compute capacity
- Data storage
- Database capacity
- Network data transfer
- Global content delivery
- Backup storage
- Monitoring and logging
- Disaster-recovery resources
- Cross-region replication
- Operational complexity

Actual AWS pricing will be investigated later when specific AWS services are
selected.

---

## 3. Option 1 — Single-Region Architecture

### Cost Characteristics

Option 1 has the simplest infrastructure footprint.

The application and data infrastructure operate primarily within one AWS
Region.

### Advantages

- Lower architectural complexity
- Fewer duplicated resources
- No full multi-region application infrastructure
- Lower operational effort
- Potentially lower baseline infrastructure cost

### Disadvantages

During breaking-news events, the primary regional infrastructure may need to
scale significantly.

If every request reaches the application infrastructure, the platform may
require substantial:

- Compute capacity
- Database capacity
- Network bandwidth
- Application scaling

This can increase cost during major traffic spikes.

### Cost Rating

**Low to Medium**

---

## 4. Option 2 — Global Distribution with Single Application Region

### Cost Characteristics

Option 2 introduces additional cost for global content distribution.

However, globally distributed caching can reduce the amount of traffic that
must reach the application origin.

### Potential Cost Benefits

Frequently requested content such as:

- News articles
- Images
- Videos
- Static website assets

can potentially be served without repeatedly using application compute and
origin infrastructure.

This can reduce pressure on:

- Application servers
- Backend systems
- Origin bandwidth

### Potential Additional Costs

The architecture introduces costs associated with:

- Global content delivery
- Cache requests
- Data transfer
- Monitoring of additional components

### Advantages

- Better ability to align application capacity with actual demand
- Reduced origin workload
- No requirement to duplicate the complete application stack globally
- Strong balance between performance and infrastructure cost

### Disadvantages

- Global content delivery introduces additional charges
- Large video delivery volumes may create significant data-transfer costs
- Cache configuration can influence cost efficiency

### Cost Rating

**Medium**

---

## 5. Option 3 — Multi-Region Application Architecture

### Cost Characteristics

Option 3 is expected to have the highest baseline infrastructure cost.

Application and supporting infrastructure must operate across multiple AWS
Regions.

### Additional Cost Areas

Potential additional costs include:

- Duplicate application capacity
- Regional database infrastructure
- Cross-region data replication
- Cross-region network transfer
- Additional monitoring
- Additional backup infrastructure
- More complex disaster-recovery operations

Some resources may need to remain available even when traffic is relatively
low.

### Advantages

- Strong global resilience
- Excellent global application performance
- Reduced dependency on one region
- Strong support for regional failover

### Disadvantages

- Highest infrastructure footprint
- Higher baseline cost
- Cross-region data-transfer costs
- Greater operational effort
- More expensive monitoring and management

### Cost Rating

**High**

---

## 6. Cost Comparison

| Cost Area | Option 1 | Option 2 | Option 3 |
|---|---|---|---|
| Baseline Infrastructure | Low | Medium | High |
| Global Delivery Cost | Low | Medium | Medium/High |
| Duplicate Regional Resources | Low | Low | High |
| Cross-Region Data Transfer | Low | Low | High |
| Operational Complexity Cost | Low | Medium | High |
| Breaking-News Origin Load | High | Lower | Distributed |
| DR Cost | Additional | Additional | Higher/Integrated |
| Overall Cost | Low-Medium | Medium | High |

---

## 7. Cost vs Business Value

The cheapest architecture is not automatically the best architecture.

For example, Option 1 may have a lower baseline cost but provides weaker global
performance and regional resilience.

Option 3 provides excellent resilience and global performance but may introduce
more infrastructure and operational cost than GlobalNews currently requires.

Option 2 introduces global distribution costs but may reduce application-origin
load while significantly improving global user performance.

Therefore, cost must be evaluated together with business value.

---

## 8. Initial Cost Conclusion

Option 1 is expected to have the lowest overall architectural cost, but it does
not fully address GlobalNews's global performance and traffic-spike challenges.

Option 3 provides the strongest global and regional architecture but has the
highest expected cost and operational overhead.

Option 2 provides a middle ground.

It introduces additional global delivery costs while avoiding the expense of
operating the complete application stack in multiple regions.

Based on the conceptual cost analysis, Option 2 currently provides the
strongest balance between cost, scalability, and global performance.

A more detailed cost estimate should be performed after the final AWS services,
traffic assumptions, storage requirements, and disaster-recovery strategy have
been defined.