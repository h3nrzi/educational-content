# Basics of API Design

## Overview

API design focuses on defining the **interface** or **contract** of an Application Programming Interface (API), specifying how clients interact with a service (e.g., Twitter, YouTube) without concerning implementation details. This section emphasizes designing **REST APIs**, the most common paradigm, and covers key principles like CRUD operations, backwards compatibility, pagination, and best practices for creating scalable, user-friendly APIs.

## Key Concepts

### 1. API Design Fundamentals

- **Definition**: The process of defining the **surface area** (or contract) of an API, specifying inputs, outputs, and behaviors without implementation details.
- **Purpose**: Enable clients (e.g., browsers, other servers) to perform actions like creating, reading, updating, or deleting resources (CRUD).
- **Key Focus**:
  - **Entities/Resources**: Nouns (e.g., `tweets`, `users`) that the API manipulates.
  - **Actions**: Defined by HTTP methods (e.g., GET, POST, PUT, DELETE) in REST, not verbs in endpoint names.
- **Example**: Twitter API for creating, reading, or deleting tweets.

### 2. REST API Design Principles

- **REST Overview**: A set of guidelines for HTTP-based APIs, emphasizing statelessness, resource-based endpoints, and standard HTTP methods/status codes.
- **Key Principles**:
  - **Resource-Based**: Endpoints represent resources (e.g., `/tweets`, `/users/{userId}/tweets`), not actions.
  - **HTTP Methods for CRUD**:
    - **GET**: Read (e.g., `GET /tweets/{id}` to fetch a tweet).
    - **POST**: Create (e.g., `POST /tweets` to create a tweet).
    - **PUT**: Update (e.g., `PUT /tweets/{id}` to update a tweet).
    - **DELETE**: Delete (e.g., `DELETE /tweets/{id}` to delete a tweet).
  - **Stateless**: Each request contains all necessary information; no server-side session state.
  - **JSON Data Format**: Commonly used for request/response bodies due to readability.
- **Example Endpoints**:
  - **Create Tweet**:
    ```http
    POST /v1/tweets
    Body: { "userId": "123", "content": "Hello, world!" }
    Response: { "tweetId": "456", "userId": "123", "content": "Hello, world!", "createdAt": "2025-09-18T08:00:00Z", "likes": 0 }
    ```
  - **Read Tweet**:
    ```http
    GET /v1/tweets/456
    Response: { "tweetId": "456", "userId": "123", "content": "Hello, world!", "createdAt": "2025-09-18T08:00:00Z", "likes": 0 }
    ```
  - **List User Tweets**:
    ```http
    GET /v1/users/123/tweets?limit=10&offset=0
    Response: [{ "tweetId": "456", ... }, ...]
    ```

### 3. Backwards Compatibility

- **Importance**: Public APIs cannot break existing clients when updated.
- **Strategies**:
  - **Optional Parameters**: Add new parameters (e.g., `parentTweetId` for replies) as optional to maintain compatibility.
    - Example: `POST /v1/tweets` with optional `parentTweetId` allows replies without breaking existing clients.
  - **Versioning**: Introduce a new API version (e.g., `/v2/tweets`) for non-compatible changes, allowing `/v1` to remain unchanged.
    - Example: Twitter API uses `/2/tweets` for version 2, coexisting with older versions.
- **Consequence of Breaking Changes**: Requiring `parentTweetId` without versioning breaks all clients not passing it.

### 4. Pagination

- **Purpose**: Manage large datasets (e.g., thousands of tweets) to prevent overwhelming clients or servers.
- **Parameters**:
  - **Limit**: Maximum number of results (e.g., `limit=10` for 10 tweets).
  - **Offset**: Starting point (e.g., `offset=10` skips first 10 tweets).
  - **Pagination Token**: Alternative to offset, returned by the server to track position (e.g., `pagination_token=xyz` for next page).
- **Example**:
  ```http
  GET /v1/users/123/tweets?limit=10&offset=0
  Response: { "tweets": [...], "nextToken": "abc" }
  GET /v1/users/123/tweets?limit=10&pagination_token=abc
  Response: { "tweets": [...], "nextToken": "def" }
  ```
- **Best Practices**:
  - Use optional `limit` with a reasonable default (e.g., 10 or 50).
  - Avoid unlimited results to protect server and client performance.
  - Prefer pagination tokens over offsets for scalability (e.g., Twitter uses tokens).

### 5. Best Practices for REST API Design

- **Clear Resource Naming**:
  - Use nouns (e.g., `/tweets`, `/users/{id}/tweets`), not verbs (e.g., avoid `/getTweets`).
  - Reflect hierarchy (e.g., `/users/{userId}/tweets` for user-specific tweets).
- **Idempotency**:
  - GET and DELETE should be idempotent (same input yields same output, no side effects).
  - Avoid using GET for creating/updating resources (e.g., `GET /tweets/create` is bad practice).
- **Rate Limiting**: Prevent abuse (e.g., Twitter limits `POST /tweets` to 200 requests per 15 minutes).
- **Documentation**: Provide clear, detailed docs for endpoints, parameters, and responses (e.g., Twitter API docs).
- **Expandability** (Stripe Example):
  - Allow clients to expand related resources (e.g., expand `customerId` to full customer object in Stripe’s payment intent).
  - Reduces need for multiple requests, similar to GraphQL’s flexibility.

### 6. Real-World Examples

- **Twitter API (v2)**:
  - **Create Tweet**: `POST /2/tweets` with JSON body (e.g., `{ "text": "Hello!" }`). Optional fields ensure backwards compatibility.
  - **User Timeline**: `GET /2/users/{id}/tweets?max_results=10&pagination_token=xyz` for paginated tweet lists.
  - Uses pagination tokens for scalability.
- **Stripe API**:
  - **Payment Intent**: `GET /v1/payment_intents/{id}?expand=customer` fetches intent with expanded customer object.
  - Well-documented, clear parameters, and expandable fields for flexibility.

### 7. System Design Relevance

- **Focus**: Design APIs that are intuitive, scalable, and backwards compatible.
- **Interview Tips**:
  - Define endpoints as functions (e.g., `createTweet(userId, content) -> Tweet`) for simplicity in coding interviews.
  - Emphasize statelessness, pagination, and compatibility when justifying designs.
  - Avoid implementation details (e.g., database logic) and focus on the API contract.
- **Trade-Offs**:
  - REST is simple and widely supported but may require multiple endpoints for complex data retrieval.
  - Pagination and versioning ensure scalability and compatibility but add design complexity.

## Key Takeaways

- **API Design**: Focuses on the interface (inputs, outputs, endpoints), not implementation.
- **REST Principles**: Stateless, resource-based, using HTTP methods and JSON; supports scalability via load balancing.
- **Backwards Compatibility**: Use optional parameters or versioning to avoid breaking clients.
- **Pagination**: Essential for large datasets, using `limit`, `offset`, or tokens.
- **Best Practices**: Clear naming, idempotency, rate limiting, and good documentation ensure usability and scalability.

## Questions for Review

1. Why is backwards compatibility critical in API design, and how does adding an optional parameter (e.g., `parentTweetId`) help achieve it?
2. Explain how pagination works in a REST API, and why might a pagination token be preferred over an offset parameter?
3. What makes a GET request idempotent, and why is it bad practice to use GET for creating resources?
4. How does the Twitter API’s user timeline endpoint (`GET /2/users/{id}/tweets`) demonstrate good REST design principles?
5. In a system design interview, how would you design a REST API for a social media platform’s “like tweet” feature, including endpoint, method, and parameters?
