# Application Layer Protocols

## Overview

Application layer protocols, built on top of transport layer protocols like TCP, are critical for developers as they define how applications communicate over networks. This section focuses on the **client-server model**, **Remote Procedure Calls (RPCs)**, and key protocols like **HTTP/HTTPS** and **WebSockets**, which are most relevant for system design interviews. These protocols enable developers to execute code remotely and build scalable applications.

## Key Concepts

### 1. Client-Server Model

- **Definition**: A model where a **client** initiates a request, and a **server** processes and responds to it.
- **Key Points**:
  - **Client**: Any entity making a request (e.g., browser, mobile app, or another server). Not limited to end-user devices.
  - **Server**: A computer (often in a data center) with a public IP address, configured to accept and fulfill requests.
  - **Example**: A browser (client) requests `youtube.com`, and the server responds with HTML; or a server (client) requests data from a third-party API (e.g., Stripe).
- **Flexibility**: Servers can act as clients when communicating with other servers, enabling complex system interactions.

### 2. Remote Procedure Call (RPC)

- **Definition**: A mechanism to execute code (a procedure or function) on a remote machine over a network, as if it were a local function call.
- **Concept**:
  - Allows clients to invoke server-side functions (e.g., `listVideos()` on YouTube’s server to fetch a user’s video feed).
  - Hides network complexity, making remote execution feel like local code execution.
- **Significance**: Most application layer protocols (e.g., HTTP) are built on the concept of RPCs, executing remote code to retrieve or manipulate data.
- **Note**: RPC is a loosely defined, somewhat outdated term, but it underpins how network-based applications function.

### 3. HTTP/HTTPS (HyperText Transfer Protocol/Secure)

- **Definition**: The primary protocol for web communication, built on TCP for reliability.
- **Key Features**:
  - **Request-Response Protocol**: Client sends a request; server responds with data or an error.
  - **Stateless**: No persistent state between client and server; all necessary data is in the request/response.
  - **HTTPS**: HTTP with **SSL/TLS** (Secure Sockets Layer/Transport Layer Security) for encryption, protecting against **man-in-the-middle (MITM)** attacks.
- **Anatomy of an HTTP Request**:
  - **Headers**:
    - **General Headers**: URL (e.g., `youtube.com/search?q=neatcode`), request method (e.g., GET), status code (e.g., 200 OK).
    - **Request Headers**: Set by client (e.g., `User-Agent: Chrome`, `Accept: text/html` for expected data format).
    - **Response Headers**: Set by server (e.g., `Content-Type: text/html` for returned data type).
  - **Body**: Contains data (e.g., form data for POST requests). GET requests typically have no body, passing data via URL query parameters.
- **HTTP Methods (CRUD Operations)**:
  - **GET**: Retrieve data (e.g., fetch YouTube homepage). Idempotent and cacheable.
  - **POST**: Create data (e.g., upload a video, submit a comment). Not idempotent, not cacheable.
  - **PUT**: Update data (e.g., edit a user profile). Idempotent.
  - **DELETE**: Remove data (e.g., delete a comment). Idempotent.
- **Status Codes**:
  - **200 OK**: Successful request.
  - **201 Created**: Resource created (e.g., after POST).
  - **400 Bad Request**: Invalid client request (e.g., malformed input).
  - **401 Unauthorized**: Client lacks permission (e.g., trying to delete another user’s video).
  - **404 Not Found**: Resource unavailable.
  - **500 Internal Server Error**: Server-side issue.
  - **502 Bad Gateway**: Issue with server communication.
  - **503 Service Unavailable**: Server temporarily down.
- **HTTPS Security**:
  - Encrypts data (e.g., password `1234` becomes hashed `xyz`) to prevent MITM attacks.
  - Essential for sensitive data (e.g., passwords on public Wi-Fi).
  - Browsers warn about non-HTTPS sites (e.g., `http://neverssl.com` shows “Not secure”).
- **Use Case**: Fetching web pages, APIs, form submissions, or any request-response interaction.

### 4. WebSockets

- **Definition**: A protocol for real-time, bidirectional communication over a single TCP connection, unlike HTTP’s request-response model.
- **Key Features**:
  - Maintains an open connection, allowing server and client to send data anytime.
  - Ideal for applications requiring continuous updates (e.g., chat apps, live notifications).
- **Comparison to HTTP**:
  - HTTP: Stateless, one request-one response, closes connection after response.
  - WebSockets: Stateful, persistent connection for ongoing data exchange.
- **Use Case**: Real-time applications like live chat, multiplayer games, or stock tickers.

### 5. Developer Focus

- **HTTP/HTTPS**: Primary protocol for web applications; developers define API endpoints (e.g., `GET /search`, `POST /videos`) and handle responses.
- **WebSockets**: Used for real-time features but less common in basic system design discussions.
- **System Design Relevance**: Understand HTTP methods, status codes, and security (HTTPS) for designing APIs; know when WebSockets are needed for real-time functionality.

## Key Takeaways

- **Client-Server Model**: Clients (browsers, servers) initiate requests; servers fulfill them. Flexible, as servers can act as clients.
- **RPC**: Concept of executing remote code, underlying protocols like HTTP.
- **HTTP/HTTPS**: Core web protocol, request-response, stateless, with methods (GET, POST, PUT, DELETE) and status codes for clear communication. HTTPS adds encryption.
- **WebSockets**: Enables real-time, bidirectional communication for specific use cases.
- **System Design**: Focus on HTTP for API design, status codes for error handling, and HTTPS for security; consider WebSockets for real-time needs.

## Questions for Review

1. How does the client-server model support communication between machines, and why is it significant that a client can be a server in some scenarios?
2. What is an RPC, and how does it relate to application layer protocols like HTTP?
3. Explain the differences between GET and POST HTTP methods, including their use cases and limitations (e.g., body vs. query parameters).
4. Why is HTTPS preferred over HTTP, and how does SSL/TLS protect against man-in-the-middle attacks?
5. When would you choose WebSockets over HTTP for an application, and provide an example of a use case where WebSockets are more suitable?
