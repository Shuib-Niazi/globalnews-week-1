# Traffic Analysis

## 1. Normal Traffic Pattern

Under normal conditions, GlobalNews traffic is relatively predictable.

The platform currently has approximately:
- 5 million registered users
- 500,000 daily active users

During normal periods, application and content demand is moderate and predictable.

## 2. Breaking-News Traffic Pattern

During major global events such as:
- Elections
- Sporting events
- Natural disasters
- Breaking-news stories

traffic can increase dramatically within minutes.

The project scenario states that traffic can increase by up to 50 times normal levels.

This creates a bursty workload where the infrastructure must respond quickly to sudden increases in demand.

## 3. Main Traffic Challenge

The primary traffic challenge is that the difference between normal traffic and peak traffic is very large.

Normal Traffic
      |
      v
Moderate Demand
      |
      v
Breaking-News Event
      |
      v
Up to 50x Traffic
      |
      v
Millions of Requests

The architecture should therefore avoid relying only on fixed infrastructure capacity.

## 4. Geographic Distribution

Users are distributed across:
- Europe
- North America
- Asia

The existing application is hosted in Europe.

This means users located farther from Europe may experience higher network latency and slower content delivery.

The future architecture must therefore consider global content delivery and geographic distribution.

## 5. Static vs Dynamic Traffic

Not all requests have the same characteristics.

### Static and Media Traffic

Examples:
- Images
- Videos
- Thumbnails
- Article assets

These resources may be requested repeatedly by many users.

They may therefore benefit from caching and distribution closer to users.

### Dynamic Application Traffic

Examples:
- User login
- Personalized content
- Search
- Comments
- Subscriptions

These requests may require application processing and access to backend systems.

Static/media traffic and dynamic traffic should not necessarily be handled in the same way.

## 6. Traffic Spike Risks

Sudden traffic increases may cause:
- Application server overload
- Increased latency
- Failed requests
- Increased bandwidth usage
- Increased infrastructure cost
- Reduced availability

The architecture must be designed to reduce the impact of these traffic spikes.

## 7. Traffic Design Requirements

Based on the workload, the architecture should support:

- Sudden increases up to 50x normal traffic
- Automatic scaling
- Global traffic distribution
- Caching
- Origin protection
- Low-latency content delivery
- Efficient handling of static and media content
- Reliable handling of dynamic application requests
- Cost efficiency during normal traffic periods

## 8. Traffic Analysis Conclusion

GlobalNews does not have a constant workload.

The system has relatively predictable normal traffic but may experience extreme and sudden demand during major news events.

The future architecture must therefore be able to scale dynamically, distribute content efficiently, and prevent unnecessary requests from overloading the application infrastructure.