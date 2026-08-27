# ADR-04 — Database Strategy

## Status

Accepted as an initial architectural decision

## Context

GlobalNews requires persistent storage for dynamic application data.

Potential data includes:

- User accounts
- Subscription information
- Comments
- Article metadata
- Categories and authors
- Application records

The project brief does not provide a complete database schema, transaction
model, or detailed access patterns.

Therefore, the database decision must remain based on reasonable architectural
assumptions and should be revisited if application requirements change.

## Decision

Use Amazon Aurora as the preferred relational database platform for the
primary application data.

The database will operate in the primary AWS Region and should be designed for
high availability across multiple Availability Zones.

Amazon S3 remains responsible for large media objects such as images and
videos.

## Why a Relational Database

Several GlobalNews data domains may contain relationships.

Examples include:

User
  |
  +---- Subscription

Article
  |
  +---- Author
  |
  +---- Category
  |
  +---- Comments

A relational model provides a natural way to represent these relationships and
supports structured querying and transactional operations where required.

## Why Amazon Aurora

### High Availability

Aurora is designed as a managed relational database service and can support a
highly available database architecture.

This fits the requirement that the application should not depend on a single
database server.

### Managed Operations

Using a managed database reduces the operational work associated with running
a database directly on application servers or self-managed EC2 instances.

### Relational Compatibility

Aurora provides a relational database model suitable for structured application
data and relationships.

### Read-Heavy Workload

GlobalNews is primarily a content-consumption platform.

Where appropriate, the architecture can later consider database read-scaling
strategies for read-heavy application workloads.

### Backup and Recovery

The database architecture must support backups and recovery as part of the
GlobalNews disaster-recovery strategy.

---

## Alternatives Considered

### Alternative 1 — Self-Managed Database on EC2

GlobalNews could install and operate its own relational database on EC2.

#### Advantages

- Full operating-system and database control
- Flexible database configuration

#### Disadvantages

- Greater operational responsibility
- Manual patching and maintenance
- More difficult high-availability management
- More backup and recovery responsibility

This option is rejected because the project favors an architecture that reduces
unnecessary operational burden.

### Alternative 2 — Amazon RDS

Amazon RDS is also a valid managed relational database approach.

#### Advantages

- Managed relational database
- Supports established relational database engines
- Backup and high-availability capabilities
- Lower operational effort than self-managed databases

#### Disadvantages

- Scaling characteristics depend on the selected database engine and design

RDS remains a valid alternative.

Aurora is preferred as the initial design direction, but the exact database
engine should be validated against application compatibility and cost before
production implementation.

### Alternative 3 — Amazon DynamoDB

DynamoDB could be used for application data with well-defined key-based access
patterns.

#### Advantages

- Fully managed
- Highly scalable
- Serverless operating model
- Suitable for predictable access patterns

#### Disadvantages

- Requires access-pattern-driven data modeling
- Relational joins are not available
- Existing application compatibility is unknown
- The project brief does not provide enough access-pattern information to
  justify redesigning all application data around DynamoDB

DynamoDB is therefore not selected as the default primary database.

It could still be appropriate for specific future workloads if their access
patterns justify it.

---

## Media Storage Separation

Large media objects should not be stored directly in the relational database.

Images
Videos
Static Assets
     |
     v
Amazon S3

Structured Application Data
     |
     v
Amazon Aurora

The database can store metadata and references to media objects rather than the
large media files themselves.

---

## Trade-Offs

Aurora introduces database costs even during periods of lower application
traffic.

The architecture must also consider:

- Database sizing
- Connection management
- Read scaling
- Backup retention
- Encryption
- Monitoring
- Database credentials
- Disaster recovery
- Application compatibility

These trade-offs are accepted because the application appears to contain
structured relational data and requires a managed production database.

## Consequences

The production architecture must later define:

- Aurora engine choice
- Instance or capacity model
- Multi-AZ architecture
- Read-scaling strategy
- Backup retention
- Encryption
- Secrets management
- Database network placement
- Security-group rules
- Monitoring
- Disaster-recovery strategy
- RPO and RTO

## Decision Limitation

The supplied project requirements do not contain a complete database schema,
query profile, transaction model, or application compatibility information.

Therefore, Aurora is a justified architectural assumption rather than a
business-mandated technology.

The decision should be revisited if detailed application requirements indicate
that another database technology is more appropriate.