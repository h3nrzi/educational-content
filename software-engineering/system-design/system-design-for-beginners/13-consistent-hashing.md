# Consistent Hashing

## Overview

Consistent hashing is a technique used in distributed systems to map requests to servers (or nodes) in a way that minimizes remapping when nodes are added or removed. It is particularly useful for load balancing, caching, and database sharding, ensuring consistent routing of requests to the same node. This section covers basic hashing, consistent hashing, its advantages over traditional hashing, and practical applications, with a focus on system design.

## Key Concepts

### 1. Basic Hashing for Load Balancing

- **Definition**: A method to distribute requests across servers by applying a hash function to a request attribute (e.g., user IP address) and mapping it to a server.
- **Process**:
  - Take a key (e.g., IP address = 6) and apply a hash function, such as modulo by the number of servers (e.g., 6 mod 3 = 0, maps to Server 0).
  - Example: IP 6 → Server 0, IP 7 → Server 1, IP 8 → Server 2 (for 3 servers labeled 0, 1, 2).
- **Assumption**: Keys (e.g., IP addresses) are uniformly distributed, ensuring roughly even load across servers.
- **Benefit**: Consistent mapping of the same user (e.g., IP 6) to the same server (Server 0).
- **Use Case**: Useful when servers have individual caches (e.g., Redis) for user-specific data, ensuring cache hits for repeated requests.
- **Limitation**: Adding or removing a server changes the hash function (e.g., mod 3 to mod 2), remapping most keys and invalidating caches.

### 2. Problem with Basic Hashing

- **Scenario**: If a server is removed (e.g., Server 2 fails, reducing from 3 to 2 servers):
  - Hash function changes (e.g., mod 3 → mod 2).
  - Example: IP 9 (9 mod 3 = 0, Server 0) → 9 mod 2 = 1 (Server 1); IP 10 (10 mod 3 = 1, Server 1) → 10 mod 2 = 0 (Server 0).
  - Result: Most users are remapped to different servers, causing cache misses and inefficiency.
- **Issue**: Cache data on original servers becomes useless, as users are routed to new servers without their cached data.

### 3. Consistent Hashing

- **Definition**: A hashing technique that maps both nodes (e.g., servers) and keys (e.g., user IPs) onto a circular space (hash ring), routing requests to the nearest node in a clockwise direction.
- **Process**:
  - **Hash Ring**: Nodes are placed on a circular space (e.g., 0–360 degrees) using a hash function (e.g., SHA).
  - **Key Mapping**: Requests (e.g., IP addresses) are hashed onto the same ring.
  - **Routing**: A request is routed to the first node encountered clockwise from its hash position.
  - Example: Servers at 0°, 120°, 240°; IP 9 maps to 100°, routes to Server at 120° (next clockwise node).
- **Key Components**:
  - **Hash Key**: Identifier for requests (e.g., IP address, user ID).
  - **Hash Function**: Industry-grade function (e.g., SHA) maps keys and nodes to the ring.
  - **Nodes**: Servers, CDN servers, or database shards.
- **Advantages**:
  - **Minimal Remapping**: When a node is removed, only requests mapped to that node’s segment are reassigned to the next clockwise node; other requests remain unchanged.
  - **Cache Efficiency**: Preserves cache hits for users mapped to unchanged nodes.
  - **Scalability**: Adding nodes only reassigns a small portion of requests, minimizing disruption.

### 4. Consistent Hashing vs. Basic Hashing

- **Basic Hashing**:
  - Hash function: Modulo by number of servers (e.g., IP mod N).
  - Issue: Adding/removing servers changes N, remapping most keys.
  - Example: Removing Server 2 (3 → 2 servers) remaps IP 9 from Server 0 to Server 1, causing cache misses.
- **Consistent Hashing**:
  - Hash function: Maps to a fixed ring (e.g., 0–360), independent of node count.
  - Example: Removing Server 2 (at 240°) reassigns only its segment’s requests (e.g., IP 11 at 250°) to Server 0 (0°); IPs 9 and 10 stay with their original servers.
- **Benefit**: Minimizes cache invalidation and ensures consistent routing.

### 5. Practical Applications

- **Load Balancing with Caching**:
  - **Scenario**: Servers with individual Redis caches (not shared).
  - **Benefit**: Consistent hashing routes the same user (e.g., IP 6) to the same server, leveraging its cache for faster responses.
  - **Example**: Twitter routing user requests to servers with user-specific cached data (e.g., profile info).
- **Content Delivery Networks (CDNs)**:
  - **Scenario**: Route requests to CDN servers consistently to maximize cache hits.
  - **Example**: Twitter’s profile pictures cached on specific CDN servers for consistent access.
- **Database Sharding**:
  - **Scenario**: Distribute user data across database nodes (shards).
  - **Benefit**: Consistent hashing ensures a user’s data (e.g., profile) is always on the same node, avoiding cross-node queries.
  - **Example**: Twitter sharding user data across nodes, routing requests to the correct shard.
- **Comparison with Rendezvous Hashing**:
  - Similar goal: Consistent mapping with minimal disruption.
  - Difference: Rendezvous hashing is simpler but less commonly discussed in system design interviews.

### 6. Handling Node Changes

- **Removing a Node**:
  - Only requests in the removed node’s segment are reassigned to the next clockwise node.
  - Example: If Server 2 (240°) is removed, requests in its segment (e.g., 200°–240°) go to Server 0 (0°), preserving other mappings.
- **Adding a Node**:
  - New node takes a portion of requests from the next clockwise node’s segment.
  - Example: Adding a server at 180° reassigns some requests from Server 2 (240°) to the new server, leaving other mappings intact.
- **Optimization**: Place nodes evenly on the ring to balance load; use virtual nodes (multiple ring positions per physical node) to improve distribution.

### 7. System Design Relevance

- **When to Use**:
  - Use consistent hashing when nodes have individual caches or data (e.g., Redis caches, database shards) to ensure consistent routing.
  - Avoid if servers are stateless and identical (e.g., REST APIs with shared caches), where simpler methods like round robin suffice.
- **Trade-Offs**:
  - **Pros**: Minimizes remapping, preserves cache hits, supports dynamic node changes.
  - **Cons**: More complex than round robin; requires a robust hash function (e.g., SHA).
- **Interview Tips**:
  - Justify consistent hashing for caching or sharding scenarios (e.g., Twitter’s user-specific caches).
  - Highlight minimal disruption when scaling nodes up or down.
  - Mention alternatives (e.g., round robin) for stateless systems.

## Key Takeaways

- **Basic Hashing**: Maps requests to servers (e.g., IP mod N), but node changes cause widespread remapping and cache misses.
- **Consistent Hashing**: Uses a hash ring to map requests and nodes, routing to the nearest clockwise node, minimizing remapping.
- **Applications**: Load balancing with per-server caches, CDNs, database sharding (e.g., Twitter’s user data).
- **Components**: Hash key (e.g., IP), hash function (e.g., SHA), nodes (servers, shards).
- **Advantages**: Preserves cache hits, supports dynamic scaling, ensures consistent routing.

## Questions for Review

1. How does basic hashing differ from consistent hashing in handling server removal, and why does this matter for caching?
2. Why is consistent hashing beneficial when each server has its own Redis cache, as opposed to a shared cache?
3. Explain how consistent hashing minimizes disruption when adding a new server to a load-balanced system like Twitter’s API.
4. How is consistent hashing applied to database sharding, and why is it critical for a social media platform’s user data?
5. In a system design interview, how would you justify using consistent hashing for a global chat application with per-server caching, compared to using round robin?
