# Limitations of HTTP and WebSockets

## Overview

Application layer protocols enable communication between clients and servers, with **HTTP/HTTPS** being the most common for web applications. However, HTTP has limitations for real-time, bidirectional communication, which **WebSockets** address. This section also briefly covers other protocols (e.g., FTP, SMTP, SSH, WebRTC) and their transport layer dependencies (TCP vs. UDP), focusing on trade-offs and use cases relevant to system design.

## Key Concepts

### 1. Other Application Layer Protocols

- **FTP (File Transfer Protocol)**:
  - **Purpose**: Transfers large files between computers.
  - **Default Port**: 21.
  - **Transport Layer**: Built on TCP for reliability.
- **SMTP (Simple Mail Transfer Protocol)**:
  - **Purpose**: Sends emails.
  - **Default Port**: 25.
  - **Transport Layer**: Built on TCP for reliable delivery.
- **SSH (Secure Shell)**:
  - **Purpose**: Remotely connects to a machine to run command-line interfaces.
  - **Transport Layer**: Built on TCP for reliability.
- **WebRTC**:
  - **Purpose**: Real-time video and audio streaming.
  - **Transport Layer**: Built on UDP for speed, as minor packet loss is acceptable in streaming.
- **Note**: TCP is more common for reliability, but UDP is used for speed-critical applications like WebRTC. Ports are not critical to memorize for system design but provide context for protocol differentiation.

### 2. Limitations of HTTP

- **Request-Response Model**:
  - HTTP is stateless; each request requires a new TCP connection (or reused in HTTP/2), followed by a response, then connection closure.
  - Suitable for fetching static data (e.g., loading `twitch.tv` or searching for videos).
- **Real-Time Challenges**:
  - For applications like chat (e.g., Twitch chat), HTTP requires **polling**:
    - Client repeatedly sends requests (e.g., every second) to check for new data (e.g., chat messages).
    - Example: At 12:00 PM, fetch messages; at 12:01 PM, fetch new messages.
  - **Issues with Polling**:
    - **Inefficient**: Frequent requests create new TCP connections, increasing latency and network overhead.
    - **Latency**: Polling intervals (e.g., 1 second) delay real-time updates, while shorter intervals (e.g., 0.1 seconds) increase server load.
    - **Scalability**: High-frequency polling from many clients can overload servers.
- **HTTP/2 Note**: HTTP/2 supports streaming, reducing the need for WebSockets in some cases, but WebSockets remain widely used and relevant for system design discussions.

### 3. WebSockets

- **Purpose**: Enables real-time, bidirectional (full-duplex) communication over a single, persistent TCP connection.
- **Key Features**:
  - **WebSocket Handshake**:
    - Client sends an HTTP request (e.g., GET) to initiate a WebSocket connection.
    - Server responds with **status code 101 (Switching Protocols)**, upgrading the connection to WebSocket.
  - **Persistent Connection**: Unlike HTTP’s request-response cycle, WebSockets maintain an open connection, allowing data to flow in both directions without new connections.
  - **Bidirectional**: Both client and server can send data anytime (e.g., client sends a chat message, server pushes new messages from other users).
  - **Push-Based**: Server pushes updates (e.g., new chat messages) to clients in real time, eliminating polling.
- **Use Case Example**:
  - **Twitch Chat**: Users send messages, and the server pushes new messages from other users in real time.
  - In the browser’s network tab, WebSocket connections (e.g., `wss://chat.twitch.tv`) show a status code 101 and continuous message updates (e.g., user messages like “What is the number in the middle of Zarya’s?”).
- **Advantages**:
  - **Efficiency**: Single connection reduces TCP handshake overhead compared to HTTP polling.
  - **Real-Time**: Immediate data push for low-latency updates.
  - **Scalability**: Fewer connections reduce server load for real-time applications.
- **Downsides**:
  - **Stateful**: Maintaining persistent connections requires server resources to track active clients.
  - **Connection Breaks**: Network issues may disrupt the connection, requiring reconnection logic.
- **Use Cases**:
  - Real-time chat (e.g., Twitch, Slack).
  - Live feeds (e.g., Twitter/X timeline updates).
  - Multiplayer games or collaborative tools requiring instant updates.

### 4. HTTP vs. WebSockets

| Feature           | HTTP/HTTPS                                           | WebSockets                       |
| ----------------- | ---------------------------------------------------- | -------------------------------- |
| **Communication** | Request-response, stateless                          | Bidirectional, stateful          |
| **Connection**    | New TCP connection per request (or reused in HTTP/2) | Single persistent TCP connection |
| **Real-Time**     | Requires polling, less efficient                     | Push-based, immediate updates    |
| **Overhead**      | Higher due to repeated TCP handshakes                | Lower after initial handshake    |
| **Use Case**      | Fetching web pages, APIs                             | Real-time chat, live feeds       |
| **Protocol**      | Built on TCP                                         | Built on TCP, initiated via HTTP |

### 5. System Design Relevance

- **HTTP**: Ideal for most web applications (e.g., loading pages, APIs) due to its simplicity and stateless nature.
- **WebSockets**: Preferred for real-time, bidirectional communication (e.g., chat, live notifications).
- **Trade-Offs**: Choose HTTP for static or infrequent data retrieval; use WebSockets for real-time needs, but account for connection management complexity.
- **Interview Focus**: Understand when to use WebSockets (e.g., chat apps) vs. HTTP (e.g., REST APIs) and justify based on application requirements.

## Key Takeaways

- **HTTP Limitations**: Inefficient for real-time applications due to polling and TCP overhead.
- **WebSockets**: Solves real-time needs with persistent, bidirectional connections, reducing latency and server load.
- **Other Protocols**: FTP, SMTP, SSH (TCP-based) and WebRTC (UDP-based) serve specific purposes but are less critical for system design interviews.
- **System Design**: Select protocols based on application needs (real-time vs. static, reliability vs. speed) and understand their trade-offs.

## Questions for Review

1. What are the main limitations of using HTTP for real-time applications like a chat system, and how does polling exacerbate these issues?
2. How does the WebSocket handshake work, and why is it necessary to establish a persistent connection?
3. Compare the efficiency of HTTP polling versus WebSockets for a Twitch-like chat application, focusing on network overhead and latency.
4. Why might WebSockets be preferred over HTTP for a live Twitter/X feed, and what challenges might arise from maintaining persistent connections?
5. Name one application layer protocol that uses UDP instead of TCP, and explain why UDP is suitable for its use case.
