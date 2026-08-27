# ADR-03 — Compute and Scaling Strategy

## Status

Accepted

## Context

GlobalNews has relatively predictable normal traffic but may experience
traffic increases of up to 50 times normal levels during major breaking-news
events.

The dynamic application layer must therefore:

- Scale when demand increases
- Reduce capacity when demand falls
- Avoid dependence on a single application server
- Replace unhealthy application instances
- Operate across multiple Availability Zones
- Integrate with load balancing and monitoring

The compute strategy should also remain understandable and maintainable.

## Decision

Use Amazon EC2 instances in an Auto Scaling Group behind an
Application Load Balancer for the primary dynamic application layer.

Application instances will be distributed across multiple Availability Zones
within the primary AWS Region.

## Reasons for the Decision

### Automatic Scaling

An Auto Scaling Group allows application capacity to increase when demand
rises and decrease when demand falls.

This is important because permanently maintaining capacity for a 50x traffic
spike would be inefficient.

### High Availability

Application instances can be distributed across multiple Availability Zones.

The failure of one instance should therefore not make the complete application
unavailable.

### Health Management

Unhealthy instances can be detected and replaced automatically.

The Application Load Balancer should route requests only to healthy
application instances.

### Separation from Static Content

EC2 instances do not need to serve every image, video, or static asset.

CloudFront and S3 will handle much of the cacheable and media workload.

This allows the application tier to focus primarily on dynamic requests.

### Operational Clarity

EC2 Auto Scaling provides a clear architecture that is relatively easy to
explain, monitor, and represent using Terraform.

## Alternatives Considered

### Alternative 1 — Fixed EC2 Instances

The application could run on a fixed number of EC2 instances.

#### Advantages

- Simple configuration
- Predictable infrastructure

#### Disadvantages

- Capacity may be insufficient during breaking-news events
- Overprovisioning would waste money during normal traffic
- Requires more manual capacity planning

This option was rejected because GlobalNews has highly variable traffic.

### Alternative 2 — Amazon ECS

The application could run as containers using Amazon ECS.

#### Advantages

- Container-based deployment
- Efficient application packaging
- Supports service scaling
- Good integration with AWS infrastructure

#### Disadvantages

- Introduces container orchestration concepts
- Requires container build and deployment processes
- Adds complexity that must be justified

ECS remains a valid alternative, especially if GlobalNews already uses
containers.

However, the project scenario does not state that the existing application is
containerized.

Therefore, EC2 Auto Scaling is selected as the simpler initial compute model.

### Alternative 3 — AWS Lambda

Dynamic application functionality could potentially use serverless functions.

#### Advantages

- No application servers to manage
- Automatic scaling
- Pay-per-use model

#### Disadvantages

- May require significant application redesign
- Not every existing application workload is naturally suited to functions
- Introduces different runtime and operational constraints

The supplied project scenario does not state that GlobalNews is designed as a
serverless application.

Therefore, Lambda is not selected as the primary application compute platform.

## Trade-Offs

EC2 requires more operating-system and instance management than a fully
serverless architecture.

The architecture must consider:

- Instance configuration
- Operating-system updates
- Scaling policies
- Health checks
- Application deployment
- Instance security
- Monitoring

These trade-offs are accepted because EC2 Auto Scaling provides a flexible and
understandable approach for the dynamic application workload.

## Consequences

The production architecture must define:

- Minimum, desired, and maximum application capacity
- Scaling metrics and policies
- Application Load Balancer health checks
- Multi-AZ instance distribution
- Instance launch configuration
- Deployment strategy
- CloudWatch monitoring and alarms
- IAM permissions
- Network placement