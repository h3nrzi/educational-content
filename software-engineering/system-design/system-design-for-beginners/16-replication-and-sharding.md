# Database Replication and Sharding

## Overview

Replication and sharding are techniques used to scale databases by distributing data and workload across multiple nodes. Replication increases read capacity and reliability by creating copies of data, while sharding splits data to improve both read and write performance for large datasets. This section covers the concepts, strategies, trade-offs, and system design implications of replication and sharding, with a focus on their application in SQL and NoSQL databases.

## Key Concepts

### 1. Replication

- **Definition**: Creating copies (replicas) of a database to increase read capacity and improve reliability.
- **Purpose**:
  - **Scale Reads**: Handle more read requests by distributing them across replicas.
  - **Reliability**: If one node fails (e.g., leader), a replica can take over or provide data.
  - **Availability**: Ensures data access even during node failures.
- **Example**: Twitter replicating user data to handle millions of read requests for profiles.

#### Leader-Follower Replication (Master-Slave)

- **Structure**:
  - **Leader Node**: Handles all write operations and replicates data to follower nodes.
  - **Follower Nodes**: Handle read operations, receiving updates from the leader.
- **Process**:
  - Clients (via an application) write to the leader; reads can go to the leader or followers.
  - Leader propagates writes to followers, ensuring data consistency over time.
- **Example**: A Twitter application writes a new tweet to the leader, which replicates it to followers; users read tweets from followers.
- **Trade-Offs**:
  - **Pros**: Scales read capacity, improves reliability (e.g., failover to a follower if the leader fails).
  - **Cons**: Followers cannot handle writes, leading to potential data inconsistency.

#### Replication Strategies

- **Asynchronous Replication**:
  - **Process**: Leader writes data and asynchronously replicates it to followers (e.g., on a schedule or with a delay).
  - **Pros**: Lower latency for writes, as the leader doesn’t wait for followers.
  - **Cons**: Temporary inconsistencies (stale data) on followers until replication completes.
  - **Example**: Twitter’s follower count may be outdated on a follower node for milliseconds.
- **Synchronous Replication**:
  - **Process**: Leader writes data and immediately replicates it to followers before confirming the transaction.
  - **Pros**: No stale data; followers are always consistent with the leader.
  - **Cons**: Higher latency, especially for geographically distributed nodes.
  - **Example**: A financial application requiring immediate consistency for transactions.
- **Use Case**: Asynchronous for applications tolerant of stale data (e.g., Twitter analytics); synchronous for strict consistency (e.g., banking).

#### Leader-Leader Replication (Multi-Master)

- **Structure**: Multiple leader nodes handle both reads and writes, replicating data among each other.
- **Process**:
  - Clients can write to any leader; leaders synchronize data asynchronously or synchronously.
  - Example: Twitter with leaders in different continents (e.g., one in North America, one in Asia) serving local users.
- **Use Case**: Geographically distributed systems where users write to local leaders (e.g., global social media).
- **Trade-Offs**:
  - **Pros**: Scales both reads and writes, supports regional independence.
  - **Cons**: High complexity, lower consistency (data may be out of sync), increased latency for synchronous replication.
  - **Example**: A user in Asia posts a tweet to the Asia leader; the North America leader may lag until synchronized.

### 2. Sharding

- **Definition**: Splitting a database’s data (e.g., table rows) across multiple nodes (shards) to improve performance and scalability.
- **Purpose**:
  - **Scale Reads and Writes**: Each shard handles a subset of data and traffic, reducing query latency.
  - **Handle Large Data**: Manages petabytes of data by distributing it across nodes.
- **Example**: Twitter sharding user data (e.g., users A–L on Shard 1, M–Z on Shard 2) to speed up queries.
- **Process**:
  - Divide a table’s rows based on a shard key (e.g., primary key, user ID).
  - Each shard runs on a separate machine to leverage more resources.
  - Queries are routed to the appropriate shard based on the shard key.

#### Sharding Strategies

- **Range-Based Sharding**:
  - **Process**: Split data by a range of values (e.g., last names A–L on Shard 1, M–Z on Shard 2).
  - **Pros**: Simple to implement, maintains data locality for range queries.
  - **Cons**: Uneven data distribution (e.g., more users with last names A–L than M–Z).
  - **Example**: Twitter sharding tweets by user ID ranges.
- **Hash-Based Sharding**:
  - **Process**: Use a hash function (e.g., consistent hashing) on a shard key (e.g., user ID) to map data to shards.
  - **Pros**: Even data distribution, minimizes remapping with consistent hashing.
  - **Cons**: Complex to implement, no range query support.
  - **Example**: Twitter using consistent hashing to shard user data across nodes.
- **Shard Key**: A column (e.g., user ID, name) used to determine which shard stores a record.

#### Challenges with Sharding

- **Complexity**: Joins across shards are slow or infeasible due to data distribution.
- **Consistency**: SQL databases lose ACID guarantees (e.g., foreign key constraints) when sharded, requiring custom logic.
- **Data Distribution**: Choosing an effective shard key is critical to avoid hotspots (e.g., uneven traffic to one shard).
- **Example**: Sharding Twitter’s user table by last name complicates joins with a tweets table if data is misaligned across shards.

### 3. SQL vs. NoSQL in Replication and Sharding

- **SQL Databases**:
  - **Replication**: Supported (e.g., MySQL leader-follower replication), but synchronous replication increases latency.
  - **Sharding**: Not natively supported; requires application-level logic to shard data and route queries.
  - **Challenges**: ACID properties (e.g., foreign key constraints, joins) complicate sharding and replication, reducing consistency.
  - **Example**: MySQL sharding requires custom code to split tables and manage cross-shard queries.
- **NoSQL Databases**:
  - **Replication**: Built-in support for leader-follower or multi-leader replication, often with eventual consistency.
  - **Sharding**: Native support (e.g., MongoDB, Cassandra), designed for horizontal scaling without relational constraints.
  - **Advantages**: No foreign keys or joins simplify sharding; eventual consistency enables scalable replication.
  - **Example**: MongoDB automatically shards collections by a shard key (e.g., user ID), distributing data across nodes.

### 4. System Design Relevance

- **Replication**:
  - **Use Case**: Scale reads for read-heavy applications (e.g., Twitter’s timeline).
  - **Strategy**: Use asynchronous replication for applications tolerant of stale data (e.g., social media); synchronous for strict consistency (e.g., banking).
  - **Reliability**: Leader-follower replication ensures failover; multi-leader supports regional scalability.
- **Sharding**:
  - **Use Case**: Handle massive datasets (e.g., Twitter’s petabytes of tweets) or high write throughput.
  - **Strategy**: Use consistent hashing for even distribution, range-based for specific query patterns.
  - **NoSQL Preference**: NoSQL databases (e.g., MongoDB, Cassandra) are designed for sharding, avoiding SQL’s relational overhead.
- **Interview Tips**:
  - Justify replication for read scalability and reliability (e.g., Twitter’s follower counts).
  - Discuss sharding for massive data (e.g., tweet storage) and shard key selection (e.g., user ID).
  - Highlight NoSQL’s native sharding vs. SQL’s custom implementation challenges.
  - Address trade-offs (e.g., eventual consistency vs. latency in replication).

## Key Takeaways

- **Replication**: Copies data across nodes to scale reads and improve reliability; leader-follower (read from followers) and multi-leader (read/write from all) models.
- **Sharding**: Splits data across nodes to scale reads/writes and handle large datasets; uses range-based or hash-based shard keys.
- **Replication Trade-Offs**: Asynchronous (low latency, stale data) vs. synchronous (high latency, consistent data).
- **Sharding Challenges**: SQL databases lose ACID guarantees

## Questions for Review

1. How does leader-follower replication differ from multi-leader replication, and when would you use each in a social media platform like Twitter?
2. What is the trade-off between asynchronous and synchronous replication in terms of consistency and latency, and provide an example scenario for each?
3. How does sharding improve query performance in a database, and why is it more straightforward to implement in NoSQL databases than SQL databases?
4. Explain how consistent hashing can be used as a sharding strategy, and why it’s beneficial for a system like Twitter’s tweet storage?
5. In a system design interview, how would you design a scalable database system for a global e-commerce platform, incorporating replication for read scalability and sharding for handling large product data?
