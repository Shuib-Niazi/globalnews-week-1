# ADR-07 — Monitoring and Observability Strategy

## Status

Accepted

## Context

GlobalNews is a high-traffic global media platform.

Normal traffic is relatively predictable, but breaking-news events can cause
traffic to increase by up to 50 times normal levels.

The operations team must therefore understand what is happening across the
platform during both normal and peak conditions.

The architecture requires visibility into:

- User traffic
- Application performance
- Application errors
- Infrastructure health
- Scaling activity
- Load balancer behavior
- Database performance
- Content delivery
- Security events
- Disaster-recovery readiness

Without appropriate monitoring, failures or performance problems may not be
detected quickly enough.

## Decision

Use Amazon CloudWatch as the primary monitoring and observability service for
the GlobalNews AWS environment.

CloudWatch will collect and monitor metrics, logs, alarms, and operational
information from supported AWS resources.

Additional service-specific logging should be enabled where appropriate.

## Monitoring Architecture

Global Users
     |
     v
CloudFront
     |
     v
Application Load Balancer
     |
     v
EC2 Auto Scaling
     |
     v
Application
     |
     v
Database

     |
     +----------------+
                      |
                      v
              Amazon CloudWatch
                      |
        +-------------+-------------+
        |             |             |
        v             v             v
      Metrics        Logs         Alarms