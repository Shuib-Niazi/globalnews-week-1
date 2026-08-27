# Assumptions

## 1. Traffic Assumptions

- Normal traffic is assumed to be moderate and relatively predictable.
- Breaking-news traffic may increase to as much as 50 times normal traffic.
- Traffic spikes may occur within minutes.
- Most users read content rather than modify it.

## 2. Geographic Assumptions

- The primary user base is distributed across Europe, North America, and Asia.
- Europe is assumed to remain an important operational region.
- International traffic is expected to increase over the next several years.

## 3. Content Assumptions

- News articles are generally small compared with images and videos.
- Images are requested frequently and usually change less often after publication.
- Video files are significantly larger than articles and images.
- Static website assets are highly cacheable.
- Dynamic content such as login, comments, search, subscriptions, and personalization requires backend processing.

## 4. Content Update Assumptions

- Breaking-news articles may be updated frequently while an event is developing.
- Published images and videos are expected to change less frequently than article text.
- New articles and media are published throughout the day.

## 5. Availability Assumptions

- The platform should tolerate individual server failures.
- The platform should tolerate an Availability Zone failure.
- A full AWS Region failure is considered a disaster-recovery scenario rather than a normal operating condition.

## 6. Scalability Assumptions

- The architecture should scale automatically when traffic rises.
- The system should not require permanent capacity sized for maximum breaking-news traffic.
- Demand may return to normal levels after major events.

## 7. Security Assumptions

- User information and administrative access must be protected.
- Media assets may require controlled access where appropriate.
- Administrative interfaces should not be publicly exposed without appropriate protection.

## 8. Cost Assumptions

- The company wants to balance performance, availability, and cost.
- Infrastructure should scale with demand where appropriate.
- Cost during normal traffic periods should remain significantly lower than cost during extreme peak traffic.

## 9. Growth Assumptions

- GlobalNews expects continued international growth over the next 3–5 years.
- The number of users, articles, images, and videos is expected to increase.
- The architecture should support this growth without requiring a complete redesign.

## 10. Project Constraints

- This is a conceptual architecture project.
- The AWS infrastructure does not need to be deployed.
- Major architecture decisions must be justified.
- At least three architecture options must be compared.
- The final architecture must be understandable and defendable.
- Terraform should be represented as an infrastructure-management design, not necessarily deployed.