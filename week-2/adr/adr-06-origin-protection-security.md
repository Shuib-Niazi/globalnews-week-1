# ADR-06 — Origin Protection and Security Strategy

## Status

Accepted

## Context

GlobalNews is a public-facing global media platform.

During major breaking-news events, traffic may increase by up to 50 times
normal levels.

The architecture must therefore protect the application origin from:

- Extremely high legitimate traffic
- Malicious requests
- Automated traffic
- Direct access that bypasses the intended public entry point
- Unnecessary repeated requests for cacheable content

The security design must also protect:

- Application infrastructure
- Media files
- User information
- Administrative interfaces

## Decision

Use Amazon CloudFront as the primary public content-delivery entry layer,
combined with AWS WAF for web-request filtering.

Application and media origins should not be unnecessarily exposed directly to
public users.

The design should use appropriate network, access-control, and origin-access
restrictions so that public traffic follows the intended path.

## High-Level Security Flow

Global Users
     |
     v
Route 53
     |
     v
CloudFront
     |
     v
AWS WAF
     |
     +-------------------------+
     |                         |
     v                         v
Amazon S3                 Application Load Balancer
Media / Static                 |
Content                        v
                         Application Layer
                              |
                              v
                           Database

## Reasons for the Decision

### 1. Origin Protection

CloudFront can absorb repeated requests for cacheable content before they
reach the application infrastructure.

This is especially important when millions of users request the same
breaking-news article or media asset.

### 2. Web Request Filtering

AWS WAF can apply rules to public HTTP requests.

Potential protections include:

- Rate-based rules
- Blocking known malicious request patterns
- Filtering unwanted requests
- Restricting suspicious traffic

### 3. Reduced Direct Exposure

The application origin should not be treated as the primary public entry point.

Public users should normally access the platform through the global delivery
layer.

This reduces unnecessary exposure of backend infrastructure.

### 4. Media Protection

Amazon S3 should not rely on broad public access where that is unnecessary.

CloudFront should retrieve media content through controlled origin access.

This allows media delivery to remain global while reducing direct public access
to the storage origin.

### 5. Private Backend Components

Database resources should remain private and should not be directly reachable
from the public internet.

Only the application components that require database access should be allowed
to communicate with the database.

### 6. Least Privilege

AWS IAM permissions should follow the principle of least privilege.

Each AWS service or administrative identity should receive only the
permissions required for its role.

## Alternatives Considered

### Alternative 1 — Direct Public Access to Application Origin

Users could connect directly to the application infrastructure.

#### Advantages

- Simpler architecture
- Fewer layers

#### Disadvantages

- Greater origin exposure
- More traffic reaches application resources
- Higher risk during traffic spikes
- Reduced opportunity for edge security controls

This alternative was rejected.

### Alternative 2 — Make S3 Media Buckets Public

Media objects could be publicly accessible directly from S3.

#### Advantages

- Simple media access
- Fewer access-control components

#### Disadvantages

- Reduces control over how media is accessed
- Users can bypass the intended distribution layer
- Harder to enforce origin-protection strategy

This approach is not preferred.

### Alternative 3 — Application-Level Security Only

All security filtering could be performed inside the application.

#### Advantages

- Centralized application logic

#### Disadvantages

- Malicious or excessive traffic already reaches the application
- Wastes compute capacity
- Poor protection during large traffic spikes

This option was rejected because security controls should exist before
requests reach backend application resources.

## Trade-Offs

Adding CloudFront, WAF, access restrictions, private networking, and IAM
controls increases configuration complexity.

The architecture must manage:

- Cache behavior
- WAF rules
- Origin access
- TLS configuration
- IAM permissions
- Security groups
- Monitoring
- Logging

Incorrect security configuration could also cause legitimate traffic to be
blocked or expose resources unintentionally.

These trade-offs are accepted because GlobalNews is a public, high-traffic
platform and the origin must be protected.

## Consequences

The production architecture must later define:

- CloudFront origins
- S3 origin-access strategy
- WAF rule design
- Application Load Balancer exposure
- Security groups
- Public and private subnets
- Database network isolation
- IAM roles and policies
- Encryption in transit
- Encryption at rest
- Logging and monitoring
- Administrative access controls

## Final Decision Statement

GlobalNews will use a layered security model in which public traffic enters
through the global distribution layer, web requests are filtered before
reaching backend infrastructure, media origins are protected from unnecessary
direct access, and backend data resources remain private.

This approach reduces origin exposure while supporting the platform's global
performance and scalability requirements.