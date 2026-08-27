# Global Performance & Latency Analysis

## 1. Purpose

This analysis evaluates how well each architecture option can provide fast and
responsive access for GlobalNews users located across Europe, North America,
and Asia.

Global performance is important because the current platform is hosted in one
European location and users outside Europe experience higher latency.

The three architecture options are:

1. Single-Region Architecture
2. Global Distribution with a Single Application Region
3. Multi-Region Application Architecture

---

## 2. Performance Challenge

GlobalNews serves users from multiple geographic regions.

If every user request must travel to one application location in Europe, users
located far from that region may experience:

- Higher network latency
- Slower page loading
- Slower media delivery
- Slower dynamic application responses

The final architecture should reduce unnecessary geographic distance between
users and frequently requested content.

---

## 3. Option 1 — Single-Region Architecture

### Performance Assessment

All users connect to the same primary AWS Region.

Users located close to the region may receive acceptable performance.

However, users in more distant locations may experience increased latency.

### Static and Media Content

Articles, images, videos, and static assets are still primarily delivered from
the same regional infrastructure.

This means users far from the region must retrieve content over longer network
distances.

### Dynamic Requests

Dynamic requests such as:

- Login
- Search
- Comments
- Subscriptions
- Personalized content

also travel to the same application region.

### Strengths

- Simple traffic path
- Easy to understand and operate
- Good performance for users close to the primary region

### Weaknesses

- Higher latency for distant users
- Poorer global content-delivery efficiency
- Longer distance for both static and dynamic requests

### Global Performance Rating

**Low to Medium**

---

## 4. Option 2 — Global Distribution with Single Application Region

### Performance Assessment

This option introduces a global distribution layer.

Cacheable content can be delivered from locations closer to users rather than
always being retrieved from the primary application region.

### Static and Media Content

This architecture is particularly effective for:

- News articles
- Images
- Videos
- Static website assets

Frequently requested content can be distributed globally.

This can significantly reduce latency for users in North America and Asia.

### Dynamic Requests

Dynamic requests still need to reach the primary application region.

Therefore, requests involving login, comments, search, subscriptions, or
personalization may still experience higher latency for distant users.

### Strengths

- Strong global performance for cacheable content
- Faster image and media delivery
- Reduced geographic distance to frequently requested content
- Reduced origin traffic
- Better user experience for international users

### Weaknesses

- Dynamic requests remain dependent on one region
- Cache strategy must be designed carefully
- Updated content requires an effective invalidation or refresh strategy

### Global Performance Rating

**High**

---

## 5. Option 3 — Multi-Region Application Architecture

### Performance Assessment

Application infrastructure operates in multiple geographic regions.

Users can potentially be directed to an application region that is closer to
them.

### Static and Media Content

Static and media content can also be distributed globally.

### Dynamic Requests

Dynamic application requests can potentially be handled in a nearby region.

This reduces latency for functionality such as:

- Login
- Search
- Comments
- Personalization
- Subscriptions

### Strengths

- Very strong global performance
- Reduced latency for static and dynamic requests
- Application processing can occur closer to users
- Strong support for future international expansion

### Weaknesses

- More complex traffic routing
- Data may need to be synchronized between regions
- More difficult operational model
- Higher infrastructure and operational cost

### Global Performance Rating

**Very High**

---

## 6. Comparison

| Architecture | Static Content Performance | Dynamic Request Performance | Global Latency | Complexity | Overall Performance |
|---|---|---|---|---|---|
| Option 1 | Medium | Medium near region, lower globally | Higher for distant users | Low | Low to Medium |
| Option 2 | High | Medium to High | Low for cached content | Medium | High |
| Option 3 | Very High | Very High | Lowest potential latency | High | Very High |

---

## 7. Initial Performance Conclusion

Option 1 does not sufficiently address GlobalNews's global-latency problem.

Option 3 provides the strongest possible global performance because both
content and application processing can be distributed geographically.

However, this comes with significantly greater cost and complexity.

Option 2 provides a strong improvement in global performance because much of
GlobalNews's workload consists of frequently requested and potentially
cacheable content.

For a media-heavy platform, Option 2 therefore provides a strong balance
between global performance and manageable operational complexity.

This is not yet the final architecture decision.