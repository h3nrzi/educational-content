# NoSQL Databases

## Overview

NoSQL databases, or "not only SQL" databases, are non-relational databases designed to address the limitations of relational database management systems (RDBMS), particularly scalability. Unlike RDBMS, NoSQL databases do not rely on SQL for querying or enforce strict relational structures like tables and foreign key constraints. This section covers the types, characteristics, trade-offs, and system design implications of NoSQL databases, with a focus on their scalability and eventual consistency.

## Key Concepts

### 1. NoSQL Databases

- **Definition**: Non-relational databases that prioritize scalability and flexibility over strict consistency and relational constraints. They do not use SQL as the primary query language, though some support SQL-like queries.
- **Purpose**:
  - Overcome RDBMS limitations, especially scalability for large-scale, high-traffic systems.
  - Support diverse data structures and dynamic schemas.
- **Comparison with RDBMS**:
  - **RDBMS**: Structured tables, fixed schemas, ACID compliance, suitable for consistent, relational data (e.g., banking).
  - **NoSQL**: Flexible schemas, eventual consistency, designed for scalability and unstructured data (e.g., social media).
- **Trade-Offs**:
  - **Pros**: High scalability, flexible schemas, fast writes/reads for specific use cases.
  - **Cons**: Sacrifices ACID properties (e.g., consistency, isolation), no standard query language, potential for stale data.

### 2. Types of NoSQL Databases

- **Key-Value Stores**:
  - **Structure**: Simple hash map-like structure mapping unique keys (e.g., user ID) to values (e.g., flat objects).
  - **Characteristics**:
    - In-memory storage (e.g., Redis, Memcached) for speed, not durable like disk-based RDBMS.
    - No relations or foreign key constraints; keys act as primary keys.
  - **Use Case**: Caching user data (e.g., Twitter session data) alongside a primary database.
  - **Example**: Redis caching user IDs mapped to session tokens.
  - **Pros**: Extremely fast reads/writes due to in-memory storage.
  - **Cons**: Limited complexity, lacks relational operations.
- **Document Stores**:
  - **Structure**: Collections of JSON-like documents with nested key-value pairs, no fixed schema.
  - **Characteristics**:
    - Documents are identified by a primary key but can have arbitrary fields (e.g., `{id: 1, name: "John", address: {city: "NY"}}`).
    - Flexible: Fields can be added/removed dynamically without schema changes.
  - **Use Case**: Storing user profiles with varying attributes (e.g., Twitter user data).
  - **Example**: MongoDB, a popular open-source document store.
  - **Pros**: Flexible schemas, scalable for writes and reads.
  - **Cons**: No joins or foreign key constraints, less suited for complex relational queries.
- **Wide-Column Stores**:
  - **Structure**: Columns organized into column families, optimized for massive write-heavy workloads.
  - **Characteristics**:
    - Flexible schemas, but some support optional schemas.
    - Ideal for time-series data or write-heavy applications (e.g., IoT sensor data).
  - **Use Case**: Logging events or analytics data with high write throughput.
  - **Example**: Cassandra, Google BigTable.
  - **Pros**: Handles massive write scale, suitable for time-series data.
  - **Cons**: Less efficient for frequent reads or updates.
- **Graph Databases**:
  - **Structure**: Nodes (entities) and edges (relationships) in a directed graph, optimized for relational queries.
  - **Characteristics**:
    - Represents complex relationships (e.g., social media follows).
    - Efficient for traversing connections (e.g., finding friends of friends).
  - **Use Case**: Social media networks (e.g., Twitter’s follower graph).
  - **Example**: Neo4j.
  - **Pros**: Excels at relationship queries, outperforms RDBMS joins for complex networks.
  - **Cons**: Less suited for non-relational, high-volume data.

### 3. Scalability in NoSQL

- **Primary Motivation**: NoSQL databases excel at horizontal scaling (adding nodes) compared to RDBMS, which are better suited for vertical scaling (upgrading hardware).
- **Challenges with RDBMS Scaling**:
  - ACID properties (e.g., consistency, isolation) make horizontal scaling complex.
  - Joins and foreign key constraints require cross-node coordination, slowing performance.
  - Example: Sharding an RDBMS (splitting data across nodes) complicates joins and constraint enforcement.
- **NoSQL Scaling**:
  - **Partitioning (Sharding)**: Split data across nodes without relational constraints, enabling independent processing (e.g., half of Twitter’s user data on Node A, half on Node B).
  - **Replication**: Create copies (replicas) of data to increase read capacity.
  - **Example**: MongoDB shards user documents across nodes; Cassandra replicates data for high availability.

### 4. Eventual Consistency and BASE

- **BASE Acronym**: Stands for **Basically Available, Soft state, Eventual consistency** (less strict than ACID).
  - **Basically Available**: System remains operational, even with stale data.
  - **Soft State**: Data may be temporarily inconsistent across nodes.
  - **Eventual Consistency**: Replicas eventually synchronize, but reads may return stale data temporarily.
- **Leader-Follower Model**:
  - **Leader Node**: Handles all writes, ensuring a single source of truth.
  - **Follower Nodes**: Replicate leader’s data asynchronously, used for reads.
  - **Process**: Writes go to the leader, which propagates updates to followers. Reads from followers may return stale data until synchronized.
  - **Example**: Twitter’s follower count may appear outdated on a follower node for milliseconds until the leader’s update propagates.
- **Trade-Off**: Sacrifices immediate consistency for higher read scalability and availability.
- **Comparison with ACID**:
  - **ACID (RDBMS)**: Ensures immediate consistency, isolation, and durability, but limits scalability.
  - **BASE (NoSQL)**: Prioritizes availability and scalability, accepting temporary inconsistencies (e.g., stale Twitter follower counts).

### 5. Practical Applications

- **Key-Value Stores**:
  - Caching session data (e.g., Redis storing Twitter user sessions).
  - Temporary data storage for high-speed access.
- **Document Stores**:
  - Storing flexible user data (e.g., MongoDB for Twitter user profiles with varying fields).
  - Content management systems with dynamic schemas.
- **Wide-Column Stores**:
  - Time-series data (e.g., Cassandra for logging Twitter analytics).
  - IoT or event logging with high write throughput.
- **Graph Databases**:
  - Social network analysis (e.g., Neo4j for Twitter’s follower/following relationships).
  - Recommendation systems (e.g., friends of friends on Facebook).

### 6. System Design Relevance

- **When to Use NoSQL**:
  - High write/read throughput (e.g., Twitter’s tweet stream).
  - Unstructured or semi-structured data (e.g., JSON-like user profiles).
  - Horizontal scaling needs (e.g., sharding for global applications).
  - Applications tolerant of eventual consistency (e.g., social media metrics).
- **When to Avoid**:
  - Strong consistency requirements (e.g., financial transactions).
  - Complex relational queries with joins (e.g., reporting systems).
- **Interview Tips**:
  - Justify NoSQL for scalability and flexibility (e.g., MongoDB for Twitter’s user data).
  - Discuss eventual consistency trade-offs (e.g., stale follower counts vs. high read capacity).
  - Combine with key-value stores (e.g., Redis) for caching alongside a primary NoSQL database.

## Key Takeaways

- **NoSQL Databases**: Non-relational, flexible schemas, designed for scalability and diverse data types.
- **Types**: Key-value (Redis), document (MongoDB), wide-column (Cassandra), graph (Neo4j).
- **Scalability**: Horizontal scaling via sharding and replication, unlike RDBMS’s vertical scaling.
- **BASE**: Prioritizes availability and eventual consistency over ACID’s strict consistency.
- **Trade-Offs**: Fast, scalable, flexible, but sacrifices relational constraints and immediate consistency.
- **Applications**: Social media (Twitter’s tweets), caching (Redis), analytics (Cassandra), social graphs (Neo4j).

## Questions for Review

1. How do NoSQL databases differ from RDBMS in terms of data structure and scalability, and why are NoSQL databases better suited for horizontal scaling?
2. What is the key difference between a key-value store and a document store, and when would you use each in a social media platform like Twitter?
3. Explain how eventual consistency works in a NoSQL database with replication, and provide an example scenario where it’s acceptable for Twitter.
4. Why are graph databases more suitable than RDBMS for handling social media relationships, such as Twitter’s follower network?
5. In a system design interview, how would you design a scalable system for a social media platform’s tweet storage using a NoSQL database, and how would you incorporate a key-value store for caching?
