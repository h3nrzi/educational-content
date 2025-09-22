# Proxies and Load Balancers

## Overview

Proxies and load balancers are essential components in distributed systems, managing network traffic to enhance performance, security, and scalability. Proxies act as intermediaries between clients and servers, while load balancers distribute traffic across multiple servers. This section covers forward and reverse proxies, load balancer types, algorithms, and their relevance in system design, focusing on high-level concepts and trade-offs.

## Key Concepts

### 1. Proxies

- **Definition**: A proxy is a middle server that intermediates between a client and a destination server, handling requests and responses on behalf of one or both parties.
- **Types**:
  - **Forward Proxy**:
    - **Function**: Hides the client’s identity from the destination server.
    - **Process**:
      1. Client sends request to forward proxy (e.g., with client’s IP address).
      2. Proxy forwards request to destination server, masking client’s identity.
      3. Server responds to proxy, which returns response to client.
    - **Use Cases**:
      - **Anonymity**: VPNs hide client IP addresses, allowing access from different regions.
      - **Access Control**: Corporate/school networks block sites (e.g., YouTube) via proxy rules.
    - **Example**: A corporate proxy blocking YouTube or a VPN accessing restricted content.
    - **Key Feature**: Protects client identity, bypasses restrictions, or enforces network policies.
  - **Reverse Proxy**:
    - **Function**: Hides the destination server’s identity from the client.
    - **Process**:
      1. Client sends request to reverse proxy, unaware of backend servers.
      2. Proxy forwards request to one of multiple backend servers.
      3. Server responds to proxy, which returns response to client.
    - **Use Cases**:
      - **Content Delivery Networks (CDNs)**: Serve cached static content (e.g., Twitter’s profile pictures).
      - **Load Balancing**: Distribute traffic across servers (see below).
    - **Example**: A CDN fetching `appleid.auth.js` from an origin server, caching it, and serving it to clients.
    - **Key Feature**: Abstracts backend servers, improving scalability and security.
- **Comparison**:
  | Feature | Forward Proxy | Reverse Proxy |
  |--------------------|------------------------------|------------------------------|
  | **Hides** | Client identity | Server identity |
  | **Client Awareness** | Knows proxy exists | Unaware of backend servers |
  | **Use Case** | VPN, corporate access control | CDN, load balancing |

### 2. Load Balancers

- **Definition**: A type of reverse proxy that distributes incoming client requests across multiple backend servers to balance load, improving scalability and reliability.
- **Purpose**:
  - **Horizontal Scaling**: Distributes traffic across multiple servers running the same application (e.g., REST APIs).
  - **High Availability**: If one server fails, traffic is rerouted to others.
  - **Performance**: Prevents any single server from becoming a bottleneck.
- **Example**: Twitter distributing `GET /tweets` requests across multiple API servers.
- **Components**:
  - **Load Balancer**: Receives client requests and forwards them to backend servers.
  - **Backend Servers**: Identical servers processing requests (e.g., stateless REST API servers).

### 3. Load Balancing Algorithms

- **Round Robin**:
  - **Process**: Distributes requests sequentially across servers (e.g., 1st to Server A, 2nd to Server B, 3rd to Server C, then repeat).
  - **Pros**: Simple, ensures even distribution for equal server capacity.
  - **Cons**: Ignores server performance differences or request complexity.
- **Weighted Round Robin**:
  - **Process**: Assigns weights to servers based on capacity (e.g., Server A gets 50% of traffic, Servers B and C get 25% each).
  - **Pros**: Accounts for varying server capabilities.
  - **Cons**: Requires configuration of weights, less dynamic.
- **Least Connections**:
  - **Process**: Routes requests to the server with the fewest active connections.
  - **Pros**: Handles varying request durations (e.g., complex requests taking longer).
  - **Cons**: Requires tracking connection counts, adding overhead.
- **Location-Based**:
  - **Process**: Routes requests to servers based on client’s geographic location (e.g., North American users to NA servers).
  - **Pros**: Reduces latency by minimizing network distance.
  - **Cons**: Requires geographic mapping, less effective for global services.
- **Hash-Based**:
  - **Process**: Hashes a request attribute (e.g., user ID, IP address) to consistently route to the same server.
  - **Pros**: Ensures consistent routing (e.g., for session affinity).
  - **Cons**: Uneven distribution if hash keys are skewed.
- **Use Case Example**:
  - Twitter might use **round robin** for evenly distributing tweet requests or **least connections** for handling variable request durations.

### 4. Load Balancer Types

- **Layer 4 (Transport Layer, TCP)**:
  - **Function**: Routes traffic based on network information (e.g., IP address, port).
  - **Pros**: Faster, as it doesn’t inspect application data.
  - **Cons**: Less flexible, cannot route based on application content (e.g., URL paths).
  - **Example**: Routing TCP traffic to servers without inspecting HTTP data.
- **Layer 7 (Application Layer, HTTP)**:
  - **Function**: Routes traffic based on application data (e.g., URL, headers).
  - **Pros**: More intelligent routing (e.g., `/tweets` to tweet servers, `/auth` to authentication servers).
  - **Cons**: Slower due to decrypting/inspecting HTTP data and establishing multiple TCP connections.
  - **Example**: Routing Twitter requests for `/tweets` to specific API servers.
- **Comparison**:
  | Feature | Layer 4 Load Balancer | Layer 7 Load Balancer |
  |--------------------|-----------------------------|-----------------------------|
  | **Layer** | Transport (TCP) | Application (HTTP) |
  | **Routing Basis** | IP address, port | URL, headers, content |
  | **Speed** | Faster | Slower |
  | **Flexibility** | Less flexible | More flexible |
  | **Use Case** | General traffic routing | Content-aware routing |

### 5. Addressing Single Point of Failure

- **Issue**: A single load balancer can fail, disrupting service.
- **Solutions**:
  - **Multiple Load Balancers**: Use replicas to handle traffic, with failover to backups if one fails.
  - **High Throughput**: Load balancers are designed for high traffic, rarely becoming bottlenecks.
- **Example**: Twitter uses multiple load balancers to ensure availability if one fails.

### 6. Real-World Examples

- **Forward Proxy**:
  - **VPN**: Hides user IP to access geo-restricted content (e.g., Netflix in another country).
  - **Corporate Proxy**: Blocks YouTube on a company network.
- **Reverse Proxy**:
  - **CDN**: Apple’s CDN serves `appleid.auth.js` for Twitter, hiding the origin server.
  - **Load Balancer**: Distributes Twitter API requests across multiple servers.
- **Tools**:
  - **NGINX**: Popular open-source reverse proxy and load balancer for HTTP routing.
  - **Cloud Providers**: AWS Elastic Load Balancer, GCP Load Balancer for out-of-the-box solutions.
  - **Maglev**: Google’s load balancing system (research paper for advanced study).

### 7. System Design Relevance

- **Proxies**:
  - **Forward Proxy**: Enhances client privacy or enforces network policies (e.g., VPN, corporate restrictions).
  - **Reverse Proxy**: Abstracts backend servers for scalability (e.g., CDNs, load balancers).
- **Load Balancers**:
  - Enable horizontal scaling of stateless servers (e.g., REST APIs).
  - Improve reliability by rerouting traffic if a server fails.
  - Optimize performance by distributing load intelligently.
- **Interview Focus**:
  - Justify proxy/load balancer usage in system design (e.g., Twitter’s API scaling).
  - Choose appropriate load balancing algorithms (e.g., round robin vs. least connections).
  - Discuss layer 4 vs. layer 7 trade-offs for specific use cases.
  - Highlight CDN as a reverse proxy for static content delivery.

## Key Takeaways

- **Proxies**: Intermediaries hiding either client (forward) or server (reverse) identity.
- **Forward Proxy**: Hides client (e.g., VPN, corporate proxy) for anonymity or access control.
- **Reverse Proxy**: Hides servers (e.g., CDN, load balancer) for scalability and abstraction.
- **Load Balancers**: Distribute traffic across servers using algorithms (e.g., round robin, least connections).
- **Layer 4 vs. Layer 7**: Layer 4 is faster but less flexible; Layer 7 is content-aware but slower.
- **Scalability**: Load balancers enable horizontal scaling; multiple replicas prevent single points of failure.

## Questions for Review

1. What is the key difference between a forward proxy and a reverse proxy, and how does a CDN function as a reverse proxy?
2. How does a load balancer improve the scalability and reliability of a system like Twitter’s API?
3. Compare round robin and least connections load balancing algorithms, and explain when each might be preferred in a social media platform.
4. Why is a Layer 7 load balancer more flexible than a Layer 4 load balancer, and what are the trade-offs in terms of performance?
5. In a system design interview, how would you incorporate a load balancer and CDN into a design for a global e-commerce platform to handle high traffic and static content delivery?
