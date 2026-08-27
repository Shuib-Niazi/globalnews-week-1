# ADR-01 — Global Content Delivery

## Status

Accepted

## Context

GlobalNews serves users across Europe, North America, and Asia.

The current platform is hosted in Europe, which means users in distant regions
may experience higher latency.

During breaking-news events, traffic can increase dramatically and millions of
users may request the same article, image, or video.

The architecture therefore needs a method to deliver frequently requested
content efficiently to users around the world.

## Decision

Use Amazon CloudFront as the global content-distribution layer.

CloudFront will sit between global users and the application/storage origins.

It will be used primarily for:

- News articles where caching is appropriate
- Images
- Videos
- CSS
- JavaScript
- Static website assets

Dynamic requests that cannot be served from cache will be forwarded to the
application origin.

## Reasons for the Decision

### Global Performance

CloudFront can serve cached content from locations closer to users.

This helps reduce latency for users located far from the primary application
region.

### Traffic Spike Handling

During breaking-news events, many users may request the same content.

Serving repeated requests from cache can reduce the amount of traffic reaching
the application origin.

### Origin Protection

CloudFront reduces unnecessary direct requests to application infrastructure.

This can help protect the origin from extremely large read traffic.

### Scalability

Content delivery can scale independently from the application compute layer.

## Alternatives Considered

### Alternative 1 — Direct Delivery from Application Servers

Users could retrieve all articles, images, videos, and static content directly
from the application infrastructure.

#### Advantages

- Simpler architecture
- Fewer components

#### Disadvantages

- Higher origin load
- Higher application bandwidth usage
- Poorer global performance
- Less effective handling of repeated traffic
- Increased risk during 50x traffic spikes

This alternative was rejected because it does not sufficiently address the
GlobalNews workload.

### Alternative 2 — Full Multi-Region Application Delivery

The entire application could run in multiple AWS Regions so users access nearby
regional infrastructure.

#### Advantages

- Excellent global performance
- Lower latency for dynamic requests
- Strong regional resilience

#### Disadvantages

- Significantly higher complexity
- More expensive
- Requires cross-region data management
- More difficult monitoring and operation

This alternative was not selected as the primary architecture because the
additional complexity is not currently justified.

## Trade-Offs

Using CloudFront introduces additional complexity around:

- Cache configuration
- Cache duration
- Cache invalidation
- Content freshness
- Public versus private content

It also introduces content-delivery and data-transfer costs.

However, these trade-offs are accepted because the service directly addresses
GlobalNews's global performance and traffic-spike requirements.

## Consequences

The final architecture must define:

- Which content is cached
- Cache duration
- Cache invalidation behavior
- How updated breaking-news articles reach users
- Which requests bypass the cache
- How the application origin is protected

These details will be developed during the production architecture design.