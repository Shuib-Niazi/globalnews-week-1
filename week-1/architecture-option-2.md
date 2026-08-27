# Architecture Option 2 — Global Distribution with a Single Application Region

## 1. Overview

This option keeps the main GlobalNews application infrastructure in one primary
AWS Region while introducing a globally distributed content-delivery layer.

The goal is to improve performance for users around the world and reduce the
number of requests that must reach the application origin.

## 2. High-Level Flow

Global Users
     |
     v
Global Distribution Layer
     |
     +----------------------+
     |                      |
     v                      v
Cached / Static Content   Dynamic Requests
                            |
                            v
                  Primary Application Region
                            |
                            v
                  Application / Data Layer

## 3. Main Characteristics

- One primary application region
- Global distribution of cacheable content
- Static and media content can be delivered closer to users
- Dynamic requests still reach the primary application region
- Reduced load on the application origin
- Lower complexity than a complete multi-region application

## 4. Advantages

### Improved Global Performance

Users can receive frequently requested content from locations closer to them,
reducing latency compared with sending every request to the primary region.

### Origin Protection

Repeated requests for the same articles, images, videos, and static assets can
be handled by the global distribution layer instead of reaching the application
infrastructure every time.

### Better Handling of Traffic Spikes

During breaking-news events, many users may request the same content.

Caching and global distribution can absorb a significant amount of this traffic
before it reaches the application layer.

### Lower Operational Complexity than Multi-Region

The main application and data infrastructure remain centralized, avoiding the
complexity of operating full application stacks and synchronized data in
multiple regions.

### Cost Efficiency

The architecture can scale globally for content delivery without permanently
duplicating the full application infrastructure in several regions.

## 5. Disadvantages

### Regional Dependency

Dynamic application functionality still depends heavily on the primary AWS
Region.

A major failure in that region could affect login, search, comments,
subscriptions, and other dynamic functionality.

### Cache Management Complexity

The architecture must define:

- Which content is cacheable
- Cache duration
- Cache invalidation
- How updated breaking-news articles are delivered quickly

### Dynamic Request Latency

Users located far from the primary application region may still experience
higher latency for requests that cannot be served from cache.

## 6. Scalability Considerations

This option can significantly reduce pressure on the application infrastructure
because frequently requested content can be served through the global
distribution layer.

The application layer still needs to scale automatically for dynamic requests.

This is especially important during breaking-news events when traffic may
increase by up to 50 times normal levels.

## 7. Availability Considerations

The global distribution layer can continue serving cached content even when
some origin components experience problems.

The primary application region should still be designed for high availability
across multiple Availability Zones.

A separate strategy is required for complete regional failure.

## 8. Security Considerations

The architecture should protect the origin so users do not unnecessarily access
application infrastructure directly.

Security controls must also protect:

- Media assets
- Application endpoints
- User information
- Administrative interfaces

## 9. Cost Considerations

This architecture may introduce additional content-delivery and data-transfer
costs.

However, it can reduce pressure on application compute and avoid the need to
permanently operate a complete application stack in multiple regions.

## 10. Disaster Recovery Considerations

The application remains dependent on one primary region.

Therefore, the architecture must include a recovery strategy for regional
failure.

Possible recovery approaches will be evaluated later in the project.

## 11. Suitability for GlobalNews

This option strongly addresses two of GlobalNews's main problems:

- Global latency
- Sudden traffic spikes

It provides better global performance and origin protection while keeping
operational complexity lower than a full multi-region architecture.

For this reason, it is a strong candidate for further evaluation, but it should
not yet be selected as the final architecture.