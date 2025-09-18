# Basics of Caching

## Overview

Caching is a critical technique in software development to improve performance by storing frequently accessed data in faster storage layers. It reduces latency, increases throughput, and minimizes network or disk access costs. This section covers caching in single and distributed systems, client-side and server-side caching, caching strategies, and eviction policies, with a focus on their relevance to system design.

## Key Concepts

### 1. Caching Fundamentals

- **Definition**: Storing a copy of data in a faster storage layer (e.g., CPU cache, RAM, disk) to reduce access time compared to slower storage (e.g., disk, network).
- **Purpose**:
  - **Lower Latency**: Accessing cache (e.g., RAM or disk) is faster than disk or network requests.
  - **Higher Throughput**: Faster reads/writes allow more operations per second.
  - **Reduced Network Cost**: Caching static content locally avoids repeated network calls.
- **Hierarchy** (Single Computer):
  - **CPU Cache**: Fastest, smallest capacity, non-persistent.
  - **RAM**: Slower than CPU cache, larger capacity, non-persistent.
  - **Disk**: Slowest, largest capacity, persistent.
- **Distributed Systems**: Caching extends to client-side (e.g., browser cache) and server-side (e.g., in-memory stores like Redis) to optimize network communication.

### 2. Client-Side Caching

- **Example**: Browser caching of static content (e.g., JavaScript, HTML, CSS) on `neatcode.io`.
  - **Scenario**: Fetching `main.js` (static JavaScript file) is cached on the client’s disk to avoid repeated network requests.
  - **Network Tab Observation**:
    - **Cached Response**: `main.js` served from disk cache (8ms) vs. network (123ms, 265KB transferred).
    - **HTTP Header**: `Cache-Control: max-age=3600` specifies cache duration (e.g., 60 minutes).
  - **Cache Hit**: Data found in cache, no network request needed.
  - **Cache Miss**: Data not in cache or expired (stale), requiring a network request.
- **Process**:
  1. Client requests static content (e.g., `GET /main.js`).
  2. Browser checks cache; if valid (not expired), serves from cache (hit).
  3. If cache miss, fetches from server, stores in cache with expiration (e.g., `max-age`).
- **Cache Ratio**: Percentage of cache hits (`hits / (hits + misses)`). Higher ratio indicates better cache efficiency.

### 3. Server-Side Caching

- **Example**: Twitter storing popular tweets in an in-memory store (e.g., Redis) to reduce disk access.
- **Scenario**:
  - Tweets stored on disk (persistent database).
  - Popular tweets (e.g., by celebrities) cached in Redis (in-memory, non-persistent, faster).
- **Process**:
  1. Client requests a tweet (`GET /tweets/{id}`).
  2. Server checks Redis; if found (cache hit), returns tweet.
  3. If cache miss, fetches from disk, stores in Redis, and returns to client.
- **Benefits**:
  - **Speed**: Memory access (e.g., 100,000 reads/sec) is faster than disk (e.g., 1,000–10,000 reads/sec).
  - **Scalability**: Caching popular data reduces database load, improving throughput.
- **Limitation**: Memory is smaller than disk, so only a subset of data (e.g., popular tweets) is cached.

### 4. Caching Strategies

- **Write-Around Caching**:
  - **Process**: Write directly to primary storage (e.g., disk), bypassing cache. On read, check cache; if miss, fetch from disk, store in cache, and return.
  - **Pros**: Only caches data that is read, avoiding unnecessary cache writes for rarely accessed data.
  - **Cons**: Initial read causes cache miss, increasing latency.
  - **Use Case**: Twitter, where most tweets are rarely read, so only frequently accessed tweets are cached.
- **Write-Through Caching**:
  - **Process**: Write to cache and primary storage simultaneously.
  - **Pros**: Ensures cache is up-to-date for reads, reducing misses.
  - **Cons**: Slower writes due to dual storage; caches all data, even if rarely read.
  - **Use Case**: Applications where most written data is frequently read.
- **Write-Back Caching**:
  - **Process**: Write to cache immediately, periodically sync to disk (lazy write).
  - **Pros**: Very fast writes, as disk I/O is delayed.
  - **Cons**: Risk of data loss if cache (memory) fails before syncing to disk.
  - **Use Case**: Non-critical data (e.g., tweet likes), where minor data loss is tolerable.
- **Trade-Offs**:
  - **Write-Around**: Efficient for read-heavy, sparse access (e.g., Twitter tweets).
  - **Write-Through**: Reliable but slower writes, suitable for frequently read data.
  - **Write-Back**: Fast but risky, best for non-critical, high-write scenarios.

### 5. Cache Eviction Policies

- **Purpose**: Remove data from cache when full to make space for new data, maximizing cache hit ratio.
- **Common Policies**:
  - **Random**: Evict random cache entries.
    - **Pros**: Simple.
    - **Cons**: Inefficient, ignores access patterns.
  - **FIFO (First-In, First-Out)**:
    - **Process**: Evict oldest data (first added to cache).
    - **Pros**: Simple, like a queue.
    - **Cons**: Ignores access frequency; may evict popular data (e.g., celebrity tweet).
  - **LRU (Least Recently Used)**:
    - **Process**: Evict data least recently accessed; move accessed data to the end (most recently used).
    - **Pros**: Retains frequently accessed data (e.g., recent popular tweets).
    - **Cons**: Requires tracking access recency.
    - **Use Case**: Twitter, where recent tweets are more likely to be read.
  - **LFU (Least Frequently Used)**:
    - **Process**: Evict data with lowest access count; track frequency of reads.
    - **Pros**: Retains most popular data (e.g., celebrity tweets with high read counts).
    - **Cons**: Older popular data may stay in cache despite no recent access.
    - **Use Case**: Systems where access frequency is stable, but less ideal for Twitter (old tweets may linger).
- **Twitter Example**:
  - **LRU Preferred**: Recent tweets are more relevant; old popular tweets may become stale.
  - **LFU Issue**: High-frequency old tweets (e.g., from a year ago) may occupy cache unnecessarily.

### 6. System Design Relevance

- **Client-Side Caching**: Reduces network requests for static content (e.g., browser caching of `main.js`).
- **Server-Side Caching**: Improves database performance by caching frequent reads in memory (e.g., Redis for tweets).
- **Caching Strategies**: Choose based on read/write patterns and data loss tolerance (e.g., write-around for Twitter, write-back for non-critical likes).
- **Eviction Policies**: Optimize cache hit ratio (e.g., LRU for Twitter to prioritize recent tweets).
- **Interview Focus**:
  - Justify caching strategy and eviction policy based on application needs.
  - Highlight trade-offs (e.g., speed vs. consistency, memory limits vs. disk persistence).

## Key Takeaways

- **Caching**: Stores data in faster storage (e.g., CPU cache, RAM, disk) to reduce latency and increase throughput.
- **Client-Side**: Browser caches static content (e.g., JavaScript) using `Cache-Control` headers to avoid network requests.
- **Server-Side**: In-memory stores (e.g., Redis) cache popular data to reduce disk access.
- **Strategies**: Write-around (read-driven), write-through (reliable), write-back (fast but risky).
- **Eviction Policies**: FIFO (simple), LRU (recency-based), LFU (frequency-based); LRU often best for dynamic data like tweets.
- **Trade-Offs**: Balance speed, consistency, and memory constraints based on application requirements.

## Questions for Review

1. How does client-side caching (e.g., browser caching of `main.js`) reduce network latency, and what role does the `Cache-Control` header play?
2. Explain the difference between write-around, write-through, and write-back caching strategies, and why write-around is suitable for Twitter’s tweet retrieval?
3. Why is LRU a better eviction policy than LFU for a Twitter-like application with frequently changing tweet popularity?
4. What is the cache hit ratio, and why is it important to maximize it in a caching system?
5. In a system design interview, how would you justify using Redis with an LRU eviction policy for caching popular tweets in a social media platform?
