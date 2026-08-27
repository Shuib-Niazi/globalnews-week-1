# Architecture Option 1 — Single-Region Architecture

## 1. Overview

This option places the primary GlobalNews application infrastructure in a
single AWS Region.

Users from Europe, North America, and Asia access the same application region.

The goal of this option is to keep the architecture relatively simple while
improving scalability and availability compared with the current traditional
single-data-centre environment.

## 2. High-Level Flow

Global Users
     |
     v
Single AWS Region
     |
     v
Application Layer
     |
     v
Storage / Database

## 3. Main Characteristics

- One primary AWS Region
- Centralized application infrastructure
- Centralized data and storage strategy
- Simpler operational model
- Lower architectural complexity
- Global users must still reach the same main region

## 4. Advantages

### Simplicity

The architecture is easier to understand, operate, monitor, and maintain than
a multi-region design.

### Lower Operational Complexity

Application deployment, monitoring, networking, and data management remain
centralized.

### Lower Cost Potential

A single-region architecture may require fewer duplicated resources than a
multi-region solution.

### Easier Data Management

Because the main application and data services remain in one region, complex
cross-region data synchronization is not required.

## 5. Disadvantages

### Global Latency

Users in North America and Asia may still experience higher latency because
dynamic requests must travel to the primary region.

### Regional Dependency

The architecture continues to depend heavily on one AWS Region.

A regional failure could cause major service disruption.

### Limited Global Resilience

Although individual infrastructure components can be made highly available
within the region, the architecture does not provide full protection against
regional failure.

### Scaling Pressure

During a major breaking-news event, extremely high traffic may still reach the
central application infrastructure.

## 6. Scalability Considerations

The architecture should support scaling within the selected region.

However, because all users rely on the same regional application environment,
very large traffic spikes could create significant pressure on the application
and backend systems.

This is especially important because GlobalNews traffic may increase by up to
50 times normal levels.

## 7. Availability Considerations

The architecture could improve availability compared with the current
single-data-centre environment by distributing resources across multiple
Availability Zones inside one region.

However, the entire system would still depend on the availability of the
selected AWS Region.

## 8. Security Considerations

The architecture would need to protect:

- Application infrastructure
- Stored media
- User information
- Administrative access
- Public application endpoints

Security controls would still need to be designed even though the architecture
is relatively simple.

## 9. Cost Considerations

This option is likely to be less expensive and simpler to operate than a
multi-region architecture.

However, the system may need significant regional capacity during breaking-news
events, and that capacity must be able to scale quickly.

## 10. Disaster Recovery Considerations

A single-region architecture has limited resilience against full regional
failure.

A separate disaster-recovery strategy would therefore be required.

## 11. Suitability for GlobalNews

This architecture improves simplicity, scalability, and availability compared
with the current environment.

However, it does not fully address the company's global-latency and regional-
resilience challenges.

For that reason, this option should be considered a valid baseline architecture
for comparison rather than automatically treated as the final solution.