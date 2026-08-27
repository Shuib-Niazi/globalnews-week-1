# Initial Architecture Comparison

## 1. Purpose

The three proposed architecture options are compared against the main GlobalNews requirements.

The goal at this stage is not to select the final architecture, but to understand the major strengths, weaknesses, risks, and trade-offs of each approach.

## 2. Architecture Options

### Option 1
Single-Region Architecture

### Option 2
Global Distribution with a Single Application Region

### Option 3
Multi-Region Application Architecture

## 3. Comparison Matrix

| Criterion | Option 1: Single Region | Option 2: Global Distribution + Single Application Region | Option 3: Multi-Region |
|---|---|---|---|
| Global Performance | Low to Medium | High for cacheable content | High |
| 50x Traffic Handling | Medium | High | Very High |
| Origin Protection | Low | High | High |
| Availability | Medium | High | Very High |
| Regional Resilience | Low | Medium | High |
| Operational Complexity | Low | Medium | High |
| Cost | Lower | Medium | Higher |
| Scalability | Medium | High | Very High |
| Disaster Recovery | Requires separate DR | Requires regional recovery strategy | Stronger regional continuity |
| Maintainability | High | Medium to High | Lower due to complexity |
| Future Global Expansion | Medium | High | Very High |

## 4. Option 1 Assessment

### Strengths

- Simple architecture
- Lower operational complexity
- Potentially lower cost
- Easier monitoring and maintenance
- Easier data management

### Weaknesses

- Higher latency for distant users
- Strong dependency on one AWS Region
- Limited protection against regional failure
- Significant traffic may still reach the central application layer

### Initial Assessment

Option 1 is the simplest approach but does not fully address GlobalNews's global performance and resilience requirements.

## 5. Option 2 Assessment

### Strengths

- Better global content performance
- Reduces application-origin traffic
- Strong support for caching
- Better handling of sudden traffic spikes
- Lower complexity than full multi-region
- Good balance between scalability and operational effort

### Weaknesses

- Dynamic requests still depend on the primary application region
- Regional failure remains an important risk
- Requires careful cache design and invalidation
- Dynamic users far from the application region may still experience latency

### Initial Assessment

Option 2 provides a strong balance between global performance, scalability, cost, and complexity.

It appears well aligned with a media platform where a large proportion of traffic may involve repeatedly requested articles, images, videos, and static assets.

## 6. Option 3 Assessment

### Strengths

- Strong global application performance
- Strong protection against regional failure
- High scalability
- Supports future geographic expansion
- Can reduce dynamic-request latency

### Weaknesses

- Highest operational complexity
- Higher infrastructure cost
- More difficult data replication
- More complicated monitoring and deployment
- Cross-region consistency and synchronization must be addressed

### Initial Assessment

Option 3 offers the strongest global resilience, but it may introduce more cost and complexity than the current GlobalNews requirements justify.

## 7. Initial Comparison Conclusion

All three options provide improvements over the current single-data-centre environment.

Option 1 provides the greatest simplicity but has important limitations in global performance and regional resilience.

Option 2 provides improved global content delivery and origin protection while maintaining a relatively manageable application architecture.

Option 3 provides the strongest global availability and regional resilience but introduces significant cost and operational complexity.

No final architecture is selected during Week 1.

The three options will be evaluated in more detail during Week 2 against:

- Scalability
- Global performance
- Availability
- Security
- Cost
- Complexity
- Disaster recovery
- Maintainability