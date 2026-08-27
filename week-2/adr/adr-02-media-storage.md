# ADR-02 — Media Storage

## Status

Accepted

## Context

GlobalNews stores millions of images and media files.

In the current environment, media files are stored directly on application
servers.

This creates several problems:

- Increased application-server storage requirements
- Higher application-server bandwidth usage
- Increased server load
- Difficult scaling
- Poor separation between application compute and media storage

The platform needs a storage approach that can scale independently from the
application layer.

## Decision

Use Amazon S3 as the primary object-storage service for media and static
content.

S3 will be used for content such as:

- Article images
- Thumbnails
- Videos
- Static website assets
- Other media files

Application servers should not be used as the primary storage location for
media files.

## Reasons for the Decision

### Separation of Responsibilities

Application compute should focus on processing dynamic application requests.

Media storage should be handled by a dedicated object-storage layer.

This separation reduces unnecessary load on application servers.

### Scalability

GlobalNews already has millions of media files and expects continued growth.

S3 is appropriate for storing a large and growing amount of object-based
content.

### Durability

Media content is valuable business data and should be stored independently from
individual application instances.

If an application instance is replaced or fails, media content should remain
available.

### Integration with Global Delivery

S3 can act as an origin for globally distributed media delivery.

This fits the selected architecture where cacheable content is distributed
globally.

## Alternatives Considered

### Alternative 1 — Store Media on Application Servers

The existing architecture stores media directly on application servers.

#### Advantages

- Simple initial setup
- Application and content are located together

#### Disadvantages

- Difficult to scale
- Application servers require large storage capacity
- Increased bandwidth consumption
- Media may be tied to individual server instances
- Application replacement can complicate data persistence
- Poor separation between compute and storage

This approach was rejected because it contributes directly to the current
scalability and operational problems.

### Alternative 2 — Shared File Storage

A shared file-storage system could provide centralized storage accessible by
multiple application servers.

#### Advantages

- Familiar filesystem model
- Multiple application servers can access common files

#### Disadvantages

- More infrastructure complexity
- Less appropriate for large-scale global object delivery
- Media delivery would still require additional architecture

This option may be suitable for some application workloads, but it is not
preferred for the large volume of public media content in GlobalNews.

## Trade-Offs

Using S3 means the application must work with object storage rather than a
traditional local filesystem.

The architecture must also define:

- Bucket access policies
- Encryption
- Versioning where appropriate
- Lifecycle policies
- Backup requirements
- Public versus private access
- CloudFront origin access

There may also be storage and data-transfer costs.

These trade-offs are accepted because S3 provides a better fit for scalable
media storage.

## Consequences

The final architecture must define:

- How applications upload media to S3
- How CloudFront retrieves media from S3
- How direct public access to S3 is controlled
- How media is encrypted
- How old content is managed
- How backups and disaster recovery are handled