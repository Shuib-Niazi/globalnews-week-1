# Business Analysis

## Global News & Media Publishing Platform

### 1. Company Overview

GlobalNews GmbH is a European digital media company that publishes breaking
news, articles, photographs, videos, and other media content to users around
the world.

The existing platform runs on traditional infrastructure located in a single
European data centre. Although the platform performs adequately during normal
traffic periods, it experiences scalability, performance, and availability
problems during major global events.

The company expects continued international growth over the next 3–5 years
and therefore requires a more scalable, highly available, secure, and
cost-conscious architecture.

### 2. Current Situation

- Approximately 5 million registered users
- Around 500,000 daily active users
- Approximately 100,000 articles
- Millions of images and media files
- Users are distributed across Europe, North America, and Asia
- The application is currently hosted in a single location
- Media files are stored on application servers
- Traffic is relatively predictable during normal periods
- Traffic can increase by up to 50× during major breaking-news events
- Users outside Europe experience higher latency
- Application servers can become overloaded during traffic spikes
- Content delivery consumes significant bandwidth
- There is currently no clear disaster recovery strategy
- Infrastructure is managed manually

### 3. Main Business Problems

#### 3.1 Traffic Spikes

Breaking-news events can cause millions of users to request the same content
within a very short period. The existing infrastructure cannot scale quickly
enough to handle these sudden increases in demand.

#### 3.2 Global Latency

The platform is hosted in Europe while its users are distributed globally.
Users in North America and Asia therefore experience slower content delivery.

#### 3.3 Inefficient Media Delivery

Images and videos are currently served directly from the application
infrastructure. This increases server load, bandwidth consumption, and
infrastructure requirements.

#### 3.4 Availability Risk

The platform depends heavily on a single infrastructure location. A major
failure at that location could make the news platform unavailable.

#### 3.5 Cost

The company does not want to permanently maintain infrastructure sized for
maximum breaking-news traffic because normal traffic is significantly lower.

### 4. Business Objectives

The future architecture should enable GlobalNews GmbH to:

- Serve millions of users reliably.
- Handle sudden 50× traffic increases.
- Improve performance for users around the world.
- Deliver articles, images, videos, and static content efficiently.
- Reduce unnecessary load on the application infrastructure.
- Improve platform availability and resilience.
- Support rapid publication and updating of news content.
- Protect media assets and user information.
- Provide monitoring and operational visibility.
- Support backup and disaster recovery.
- Control infrastructure costs during both normal and peak traffic.
- Support future international expansion.
- Prepare the infrastructure for management through Terraform.