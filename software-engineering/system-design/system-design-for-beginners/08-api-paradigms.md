# Application Programming Interfaces (APIs)

## Overview

An **Application Programming Interface (API)** is a general term for any interface that allows software components to communicate. In system design, APIs typically refer to **HTTP-based interfaces** for client-server or server-server communication. This section covers three modern API paradigms: **REST**, **GraphQL**, and **gRPC**, focusing on their characteristics, use cases, and trade-offs, which are critical for system design interviews.

## Key Concepts

### 1. What is an API?

- **Definition**: An interface for programmatically accessing or manipulating data or functionality.
  - **Examples**:
    - **Local Storage API**: Browser API for persistent storage (e.g., saving light/dark theme preferences on `youtube.com`).
    - **Object Methods**: In object-oriented programming, methods of a class (e.g., `getName()`, `setAddress()`) are an API.
    - **HTTP APIs**: External services (e.g., YouTube’s API for fetching videos), the focus of system design.
- **System Design Context**: APIs refer to HTTP-based interfaces for serving user requests, enabling scalable client-server interactions.

### 2. REST APIs (Representational State Transfer)

- **Definition**: A set of guidelines (not a protocol) for building HTTP-based APIs, emphasizing statelessness and resource-based design.
- **Key Features**:
  - **Stateless**: Each request contains all necessary information; no server-side session state is maintained.
    - **Example**: Fetching videos from `youtube.com/videos?limit=10&offset=0` includes pagination details in the URL, avoiding server-side tracking.
  - **Resources**: APIs are organized around nouns (e.g., `/videos`, `/comments`), not verbs. Actions are defined by HTTP methods (GET, POST, PUT, DELETE).
  - **HTTP Integration**: Uses HTTP methods, status codes (e.g., 200 OK, 404 Not Found), and JSON as the primary data format.
  - **Pagination**: Uses query parameters (e.g., `?limit=10&offset=10`) to manage large datasets without server-side state.
- **Benefits**:
  - **Scalability**: Statelessness allows horizontal scaling via load balancers, as any server can handle any request without session data.
  - **Standardization**: Familiar HTTP conventions (e.g., status codes, methods) make REST widely adopted and easy to use.
  - **Caching**: GET requests are idempotent and cacheable, improving performance.
- **Downsides**:
  - **Overfetching**: Returns all fields of a resource (e.g., fetching a user’s full profile when only `username` is needed).
  - **Underfetching**: Requires multiple requests for related data (e.g., separate calls for videos, comments, and user profiles).
  - **JSON Overhead**: Human-readable JSON increases data size, impacting performance.
- **Use Case**: General web applications, APIs for fetching or updating resources (e.g., YouTube’s video or comment APIs).
- **Data Format**:
  - **JSON (JavaScript Object Notation)**: Human-readable, flexible (supports strings, numbers, booleans, nested objects, arrays), but larger due to text-based format.
  - **Example**:
    ```json
    {
      "firstName": "John",
      "address": {
        "street": "123 Main St",
        "city": "Boston"
      },
      "phoneNumbers": [
        { "type": "home", "number": "555-1234" },
        { "type": "office", "number": "555-5678" }
      ],
      "email": null
    }
    ```

### 3. GraphQL APIs

- **Definition**: A query-based API paradigm (created by Facebook in 2015) built on HTTP POST requests, allowing clients to request specific data fields.
- **Key Features**:
  - **Single Endpoint**: One endpoint (e.g., `/graphql`) handles all queries and mutations.
  - **Query**: Read operations to fetch specific fields (e.g., `username`, `profilePicture` for comments).
  - **Mutation**: Write operations (e.g., create, update, delete resources).
  - **Schema-Driven**: Strict schema defines available fields, ensuring predictable responses.
  - **Flexible Queries**: Clients specify exact fields needed, avoiding overfetching/underfetching.
- **Benefits**:
  - **No Overfetching**: Clients request only required fields (e.g., `username` instead of full user profile).
  - **No Underfetching**: Single query retrieves related data (e.g., videos, comments, and user info in one request).
  - **Improved Performance**: Less data transferred over the network.
  - **Tooling**: GraphQL Explorer (e.g., SpaceX API) allows interactive query building.
- **Downsides**:
  - **Non-Idempotent**: Uses POST requests, which are not cacheable, unlike REST GET requests.
  - **Complexity**: Schema management and query validation add server-side complexity.
  - **Learning Curve**: Less intuitive than REST for developers unfamiliar with GraphQL syntax.
- **Use Case**: Applications needing flexible data retrieval (e.g., social media apps fetching comments, users, and posts in one query).
- **Example** (SpaceX GraphQL API):
  - Query for past 10 launches, specifying only `mission_name` and `rocket.first_stage.cores.status`:
    ```graphql
    query {
      launchesPast(limit: 10) {
        mission_name
        rocket {
          first_stage {
            cores {
              status
            }
          }
        }
      }
    }
    ```

### 4. gRPC APIs

- **Definition**: A high-performance API framework (created by Google in 2016) built on HTTP/2, using **protocol buffers** for data serialization.
- **Key Features**:
  - **Protocol Buffers**: Schema-driven, binary format for compact, fast data transfer (unlike JSON’s text-based format).
  - **HTTP/2**: Supports streaming (client-to-server, server-to-client, bidirectional), similar to WebSockets.
  - **Schema-Driven**: Defines strict data structures (e.g., `message SearchRequest { string query = 1; }`) for requests and responses.
  - **RPC-Based**: Endpoints are actions (e.g., `SayHello`), not resources, unlike REST.
- **Benefits**:
  - **Performance**: Binary protocol buffers reduce data size, making gRPC faster than REST.
  - **Streaming**: Supports one-way and bidirectional streaming, ideal for real-time applications.
  - **Multi-Language Support**: Generates client/server code for languages like Python, Java, Go.
- **Downsides**:
  - **Browser Incompatibility**: Requires a proxy (e.g., gRPC-Web) for browser use due to HTTP/2 control limitations.
  - **Complexity**: Protocol buffers and schema management add development overhead.
  - **Less Tooling**: Newer and less standardized than REST, with weaker community support.
  - **Error Handling**: Uses custom error messages, not HTTP status codes, requiring custom client-side handling.
- **Use Case**: Server-to-server communication (e.g., microservices) where performance and streaming are critical.
- **Example** (Protocol Buffer Schema):
  ```proto
  syntax = "proto3";
  message SearchRequest {
    string query = 1;
    int32 page_number = 2;
    int32 results_per_page = 3;
  }
  service CreatorService {
    rpc SayHello (HelloRequest) returns (HelloReply);
  }
  message HelloRequest {
    string name = 1;
  }
  message HelloReply {
    string message = 1;
  }
  ```

### 5. Trade-Offs Comparison

| Feature             | REST                        | GraphQL                     | gRPC                               |
| ------------------- | --------------------------- | --------------------------- | ---------------------------------- |
| **Protocol**        | HTTP (1.1, 2, 3)            | HTTP (POST only)            | HTTP/2                             |
| **Data Format**     | JSON (text, larger)         | JSON (text, flexible)       | Protocol Buffers (binary, compact) |
| **Schema**          | Optional, flexible          | Strict schema               | Strict schema (protocol buffers)   |
| **Caching**         | GET requests cacheable      | POST requests not cacheable | Not cacheable, custom errors       |
| **Use Case**        | General web APIs            | Flexible data retrieval     | Server-to-server, high-performance |
| **Real-Time**       | Polling or HTTP/2 streaming | Limited (subscriptions)     | Native streaming support           |
| **Browser Support** | Native                      | Native                      | Requires proxy (gRPC-Web)          |
| **Complexity**      | Simple, widely supported    | Moderate, schema-driven     | Complex, less tooling              |

### 6. System Design Relevance

- **REST**: Default choice for web APIs due to simplicity, standardization, and browser compatibility. Ideal for resource-based applications (e.g., YouTube APIs).
- **GraphQL**: Use for complex applications needing flexible data retrieval (e.g., social media dashboards). Consider non-cacheable POST requests.
- **gRPC**: Best for performance-critical, server-to-server communication (e.g., microservices). Avoid for browser-based apps without proxies.
- **Interview Focus**: Understand REST deeply, know GraphQL/gRPC trade-offs (e.g., performance vs. complexity), and justify choices based on application needs.

## Key Takeaways

- **APIs**: Interfaces for programmatic communication, typically HTTP-based for system design.
- **REST**: Stateless, resource-based, widely adopted, scalable, but prone to overfetching/underfetching.
- **GraphQL**: Flexible, single-endpoint queries/mutations, solves overfetching/underfetching, but less cacheable.
- **gRPC**: High-performance, schema-driven, ideal for server-to-server streaming, but complex and browser-limited.
- **Trade-Offs**: Balance simplicity (REST), flexibility (GraphQL), and performance (gRPC) based on application requirements.

## Questions for Review

1. What does it mean for a REST API to be stateless, and how does pagination (e.g., `?limit=10&offset=10`) support scalability in a RESTful architecture?
2. How does GraphQL address the problems of overfetching and underfetching in REST APIs, and what is a key downside of using POST requests for GraphQL?
3. Why is gRPC faster than REST, and what makes it unsuitable for direct use in browser-based applications?
4. Compare the use of JSON in REST APIs with protocol buffers in gRPC, focusing on their impact on performance and readability.
5. In a system design interview, how would you choose between REST, GraphQL, and gRPC for a real-time chat application versus a video streaming API?
