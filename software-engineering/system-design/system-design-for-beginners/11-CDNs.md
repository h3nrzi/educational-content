# Content Delivery Networks (CDNs)

## Overview

Content Delivery Networks (CDNs) are distributed systems designed to cache static content closer to end users, reducing latency, improving availability, and decreasing load on origin servers. This section covers the purpose, functionality, types (push and pull CDNs), and practical examples of CDNs, emphasizing their role in system design for delivering static content efficiently.

## Key Concepts

### 1. What is a CDN?

- **Definition**: A network of geographically distributed servers (CDN servers) that cache static content to serve it faster to users, reducing the distance data travels compared to fetching from a central origin server.
- **Purpose**:
  - **Lower Latency**: Content is served from a nearby CDN server, not the origin server (e.g., reducing travel distance for users worldwide).
  - **Higher Availability**: Multiple CDN servers ensure redundancy; if one fails, another can serve the content.
  - **Reduced Origin Server Load**: Offloads static content delivery, allowing the origin server to focus on dynamic requests.
- **Components**:
  - **Origin Server**: Central server hosting the application and dynamic content.
  - **CDN Servers**: Distributed servers caching static content (e.g., JavaScript, images, videos).
- **Limitation**: CDNs only cache **static content** (unchanging data like JavaScript, images, or videos), not dynamic content or application logic.
  - **Note**: Emerging **edge servers** allow running code closer to users, but this is beyond the scope of traditional CDNs.

### 2. Static Content in CDNs

- **Examples**:
  - **JavaScript**: E.g., `main.js` on `neatcode.io`, identical for all users unless updated.
  - **Images**: E.g., Twitter profile pictures, which remain static until changed.
  - **Videos**: E.g., a YouTube video file, consistent across users.
- **Why Cache Static Content?**
  - Unchanging, so safe to cache globally without frequent updates.
  - Reduces network requests to the origin server, improving performance.
- **Example (Twitter)**:
  - Profile pictures are static and can be cached on CDNs.
  - JavaScript for Apple authentication (`appleid.auth.js`) is hosted on a CDN, not Twitter’s origin server.

### 3. Types of CDNs

- **Push CDN**:
  - **Process**: When content is created or updated (e.g., user uploads a profile picture), the origin server pushes it to all CDN servers immediately.
  - **Pros**:
    - Ensures content is available globally right away.
    - Ideal for frequently accessed content (e.g., popular images).
  - **Cons**:
    - Increases storage and bandwidth usage, as all CDNs store the content regardless of demand.
    - Not ideal if content is rarely accessed in some regions.
  - **Use Case**: Twitter pushing a celebrity’s profile picture to all CDNs for global access.
- **Pull CDN**:
  - **Process**:
    1. Client requests content from the nearest CDN server.
    2. If cached (hit), CDN serves it; if not (miss), CDN fetches from the origin server, caches it, and serves it.
    3. Subsequent requests to the same CDN are hits.
  - **Pros**:
    - Only caches content demanded by users in a region, saving resources.
    - Acts as a proxy, fetching from the origin server on misses.
  - **Cons**: Initial cache miss introduces latency.
  - **Use Case**: Twitter profile pictures cached only in regions where users request them.
- **Comparison**:
  | Feature | Push CDN | Pull CDN |
  |--------------------|-------------------------------|-------------------------------|
  | **Content Distribution** | Pushed to all CDNs immediately | Fetched on demand (cache miss) |
  | **Storage Usage** | Higher (all CDNs store data) | Lower (only demanded data cached) |
  | **Initial Latency** | Low (content pre-cached) | Higher (miss requires origin fetch) |
  | **Use Case** | Frequently accessed content | Region-specific content |

### 4. CDN Mechanics

- **Workflow**:
  1. Client requests content (e.g., `GET appleid.auth.js`).
  2. Request routes to the nearest CDN server (based on geographic proximity).
  3. CDN checks cache:
     - **Hit**: Serves content directly.
     - **Miss**: Fetches from origin server, caches it, and serves it.
  4. HTTP headers (e.g., `Cache-Control: public, max-age=86400`) determine caching behavior.
- **Cache-Control Header**:
  - **Public**: Allows CDNs to cache content (unlike `private`, which restricts caching to client browsers).
  - **Max-Age**: Specifies cache duration (e.g., 86,400 seconds = 1 day).
  - Example: Twitter’s Apple authentication JavaScript has `Cache-Control: public, max-age=86400`, enabling CDN caching.
- **Benefits**:
  - **Latency**: Shorter distance to CDN (milliseconds) vs. origin server (hundreds of milliseconds).
  - **Availability**: Redundant CDN servers ensure reliability.
  - **Scalability**: Offloads static content delivery from origin server.

### 5. Real-World Example

- **Twitter (via Dev Tools)**:
  - **Observation**: HTML in the Elements tab shows a JavaScript file (`appleid.auth.js`) hosted on Apple’s CDN (`https://appleid.cdn-apple.com`).
  - **Network Tab**: Request to `appleid.auth.js` shows `Cache-Control: public, max-age=86400`, confirming CDN caching.
  - **Why CDN?**:
    - Static JavaScript is identical for all users, safe to cache globally.
    - Reduces load on Twitter’s origin server, serving it from Apple’s CDN closer to users.
- **Analogy**: Like shipping bananas from a farm (origin server) to local grocery stores (CDNs) for faster access by customers.

### 6. System Design Relevance

- **Use in Design**: CDNs are critical for serving static content in web applications, improving performance and reliability.
- **When to Use**:
  - Cache static assets (e.g., images, videos, JavaScript, CSS) to reduce latency and server load.
  - Avoid for dynamic content (e.g., user-specific Twitter feed), which requires origin server processing.
- **Trade-Offs**:
  - **Push CDN**: Ensures low latency but wastes storage for unused content.
  - **Pull CDN**: Efficient for region-specific demand but introduces initial latency on cache misses.
- **Interview Focus**:
  - Justify CDN usage for static content delivery (e.g., Twitter profile pictures).
  - Explain push vs. pull CDNs based on content access patterns.
  - Highlight `Cache-Control` headers for controlling CDN behavior.

## Key Takeaways

- **CDNs**: Distribute static content to servers closer to users, reducing latency and origin server load.
- **Static Content**: JavaScript, images, videos (e.g., Twitter profile pictures, Apple authentication JavaScript).
- **Push CDN**: Proactively distributes content to all CDNs; best for globally accessed data.
- **Pull CDN**: Caches content on demand; efficient for region-specific data.
- **Cache-Control**: `public` allows CDN caching; `max-age` sets duration.
- **Benefits**: Lower latency, higher availability, and scalability for static content delivery.

## Questions for Review

1. How do CDNs reduce latency for end users, and why are they limited to caching static content?
2. Compare push and pull CDNs in terms of their operation, advantages, and suitable use cases for a social media platform like Twitter.
3. What is the role of the `Cache-Control: public` header in enabling CDN caching, and how does it differ from `private`?
4. Why might a pull CDN be preferred over a push CDN for Twitter profile pictures in regions with varying user interests?
5. In a system design interview, how would you integrate a CDN into a video streaming service to optimize delivery of video thumbnails and JavaScript files?
