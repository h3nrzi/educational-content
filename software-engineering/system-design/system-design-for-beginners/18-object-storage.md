# Object Storage

## Overview

Object storage is a modern storage mechanism designed for storing large, unstructured data like images, videos, and backups. It differs from traditional file systems (with hierarchies like folders) and databases (with querying and schemas). Popular examples include AWS S3, Google Cloud Storage, and Azure Blob Storage. Object storage provides fast access to flat, immutable objects via unique keys, making it ideal for scalable, long-term storage in distributed systems.

## Key Concepts

### 1. Object Storage vs. Other Storage

- **Definition**: A flat storage system for binary large objects (blobs), such as media files or backups. Objects are identified by globally unique keys, with no folder hierarchy.
- **Comparison**:
  - **File Systems**: Hierarchical (folders/files); object storage simulates folders via key prefixes (e.g., "folder/file.jpg" is a single key).
  - **Databases**: Structured with schemas, indexes, and queries (e.g., SQL joins); object storage is unstructured, flat, and query-free.
  - **Blob Storage**: Predecessor to object storage; focuses on binary data without metadata or access features.
- **Characteristics**:
  - **Flat Structure**: No true hierarchy; all objects stored in a single namespace.
  - **Immutable**: Objects can be written once and read multiple times but not updated in place (requires overwriting with a new object).
  - **Unique Keys**: Globally unique names (e.g., across all S3 users) prevent duplicates, like hash map keys.
  - **HTTP Interface**: Access via HTTP requests (e.g., GET /object-key for retrieval); no SQL or complex queries.
- **Trade-Offs**:
  - **Pros**: Fast access (O(1) via key), scalable for massive data, low-cost long-term storage.
  - **Cons**: No partial updates (immutable), no built-in search/querying (relies on metadata or external indexes).

### 2. How Object Storage Works

- **Storage Mechanism**:
  - Objects are blobs (binary large objects): Large files (e.g., gigabytes of videos).
  - Stored with metadata (e.g., tags, timestamps) but no schema.
  - Access: Use the key to retrieve (e.g., HTTP GET to S3 endpoint).
- **Illusion of Folders**: Prefixes in keys simulate hierarchy (e.g., "images/profile.jpg" appears as "images/" folder with "profile.jpg" file).
- **Security**: Configurable access (e.g., private by default; use signed URLs for temporary public access).
- **Durability**: High redundancy (e.g., S3 replicates across data centers for 99.999999999% durability).

### 3. Use Cases

- **Large File Storage**:
  - **Media**: Images, videos (e.g., Twitter profile pictures, YouTube thumbnails).
  - **Backups**: Database snapshots or logs.
  - **Long-Term Archiving**: Rarely accessed data (e.g., historical images).
- **Why Not Databases?**:
  - Databases slow down with large blobs (e.g., gigabyte videos increase query times and storage costs).
  - No need for querying blobs; access by key (e.g., "user123/profile.jpg").
  - Example: Store video metadata (title, tags) in a database; video file in object storage.
- **Integration**:
  - Application fetches blob via HTTP (e.g., S3 URL), serves to user.
  - Example: Twitter uploads profile image to S3, stores key in database; app retrieves image via key.

### 4. Advantages and Trade-Offs

- **Advantages**:
  - **Scalability**: Handles petabytes without hierarchy overhead.
  - **Speed**: O(1) access via key, like a hash map.
  - **Cost-Effective**: Optimized for infrequent access (e.g., cheaper than databases for blobs).
  - **Global Uniqueness**: Prevents naming conflicts across users.
- **Trade-Offs**:
  - **Immutability**: No in-place edits; overwrite entire object (e.g., updating a video requires a new upload).
  - **No Querying**: Cannot search/filter blobs directly (use external indexes or metadata in a database).
  - **No Hierarchy**: Flat structure; simulated folders add complexity for organization.
- **Comparison to Alternatives**:
  - **Databases**: Better for structured, queryable data; poor for large blobs.
  - **File Systems**: Hierarchical but less scalable for global, distributed access.

### 5. System Design Relevance

- **When to Use Object Storage**:
  - Large, unstructured files (e.g., user uploads in social media).
  - Long-term storage with infrequent access (e.g., backups).
  - Integration with CDNs for global delivery (e.g., S3 + CloudFront for videos).
- **Design Patterns**:
  - Store metadata in a database (e.g., SQL for image tags); blobs in object storage (e.g., S3 key).
  - Use signed URLs for secure, temporary access (e.g., private video links).
- **Interview Tips**:
  - Recommend object storage for media/backups in high-scale systems (e.g., Twitter images).
  - Discuss immutability (e.g., version objects for updates).
  - Combine with databases for metadata (e.g., MongoDB keys + S3 blobs).
- **Examples**:
  - **Twitter**: Profile images in S3 (key: "user123/profile.jpg"); metadata (e.g., upload date) in database.
  - **YouTube**: Videos in object storage; thumbnails/metadata in database.

## Key Takeaways

- **Object Storage**: Flat, immutable storage for large blobs (e.g., images, videos) via unique keys and HTTP.
- **No Hierarchy**: Simulated folders via key prefixes; globally unique names prevent duplicates.
- **Use Cases**: Media, backups; better than databases for large files due to speed and cost.
- **Trade-Offs**: Fast access but no updates/queries; immutable objects require overwrites.
- **System Design**: Pair with databases for metadata; integrate with CDNs for global delivery.

## Questions for Review

1. How does object storage differ from traditional file systems and databases in terms of structure and access, and why is it optimized for large blobs like videos?
2. Explain the immutability trade-off in object storage, and how would you handle updating a file (e.g., a user profile image) in a system like Twitter?
3. Why are globally unique keys important in object storage services like AWS S3, and how do they compare to hash map keys?
4. When designing a social media platform, why would you store user-uploaded images in object storage rather than a database, and how would you integrate metadata?
5. In a system design interview, how would you use object storage with a CDN to deliver videos in a global video-sharing app, and what are the key benefits?
