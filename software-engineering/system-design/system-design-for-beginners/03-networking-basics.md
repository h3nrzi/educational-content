# Basics of Networking for System Design

## Overview

Networking enables communication between computers (e.g., client and server) over the internet or local networks. For system design interviews, understanding the fundamentals of how data is sent, received, and managed is critical. The key focus is on moving data between machines using protocols, IP addresses, and ports, with an emphasis on practical application over deep technical details.

## Key Concepts

### 1. IP Addresses

- **Purpose**: Uniquely identify machines on a network, similar to addresses for physical mail.
- **Types**:
  - **Public IP Address**: Required for servers to be accessible over the public internet (e.g., for clients to send requests).
  - **Private IP Address**: Used within local networks (LANs) for devices behind a router. Only the router needs a public IP.
  - **Static vs. Dynamic**:
    - **Static IP**: Fixed address, ideal for servers to ensure consistent client access.
    - **Dynamic IP**: Changes over time, common for clients; managed by **Dynamic DNS** to maintain connectivity.
- **IP Versions**:
  - **IPv4**: 32-bit address (e.g., 192.168.0.1), supports ~4 billion unique addresses. Limited due to global device growth.
  - **IPv6**: 128-bit address, supports vastly more addresses, less common but growing in use.
- **Format**: IPv4 written as four 8-bit numbers (0–255) separated by dots (e.g., 255.255.255.255 max).

### 2. Data Transmission

- **Packets**: Data sent over networks is broken into packets, each containing:
  - **Header**: Metadata (e.g., source/destination IP addresses).
  - **Payload**: Actual data (e.g., part of a file, HTTP request).
- **Internet Protocol (IP)**:
  - Defines rules for sending packets between machines using IP addresses.
  - Includes **IP header** with source/destination IPs but lacks sequencing or reliability features.
- **Transmission Control Protocol (TCP)**:
  - Builds on IP to handle large data transfers.
  - **TCP Header**: Includes sequence numbers to reassemble packets in the correct order at the destination.
  - Ensures reliable delivery by managing packet order and retransmission if packets are lost.
- **Analogy**: Sending a book via mail:
  - IP = Sending individual envelopes with "from" and "to" addresses (metadata).
  - TCP = Numbering pages (sequence numbers) to reassemble the book correctly.

### 3. Application Layer Protocols

- **HTTP/HTTPS**:
  - **HTTP**: Application protocol for web communication, default port **80**.
  - **HTTPS**: Secure version of HTTP, default port **443**.
  - Data (e.g., HTML, JSON) lives in the packet’s payload, used by applications.
- **Layered Model**:
  - **Application Layer**: HTTP/HTTPS (what developers interact with).
  - **Transport Layer**: TCP (ensures reliable data delivery).
  - **Network Layer**: IP (routes packets between machines).
- **Developer Focus**: Application data (e.g., HTTP requests/responses) is primary; IP/TCP handled by underlying systems.

### 4. Ports

- **Purpose**: Channels for communication, allowing multiple services on one machine.
- **Details**:
  - 16-bit value, ~65,000 possible ports per machine.
  - Each service (e.g., web server, SSH) uses a specific port.
  - **Default Ports**: HTTP (80), HTTPS (443), SSH (22).
  - Example: Accessing `google.com` uses HTTPS on port 443 implicitly.
- **Behavior**:
  - Servers listen on specific ports (e.g., Angular app on port 4200).
  - Clients receive responses on random ports assigned by the system.
  - Only one application can use a port at a time (e.g., port 4200 conflict if reused).
- **Localhost**: Special IP `127.0.0.1` for the local machine (e.g., `localhost:4200` for local apps).

### 5. Networking in Practice

- **Client-Server Communication**:
  - Client sends a request to a server’s public IP and port (e.g., `google.com:443`).
  - Server responds to client’s IP and a random port.
- **Routers and LANs**:
  - Devices in a local network (LAN) use private IPs assigned by the router.
  - Router’s public IP handles external communication.
- **Challenges**:
  - Firewalls/port forwarding (beyond scope) may restrict access to private IPs.
  - Dynamic IPs require systems like Dynamic DNS for consistent server access.

## Key Takeaways

- **Networking Core**: Machines communicate using IP addresses, ports, and protocols (IP for routing, TCP for reliability, HTTP/HTTPS for application data).
- **System Design Relevance**: Developers focus on application data and high-level networking (e.g., choosing ports, ensuring server accessibility) rather than low-level packet details.
- **Scalability/Availability**: Public IPs and proper port management ensure servers are reachable; TCP ensures reliable data transfer for large-scale systems.

## Questions for Review

1. What is the role of an IP address in networking, and how do IPv4 and IPv6 differ in addressing capacity?
2. How do IP and TCP protocols work together to enable reliable data transfer between a client and server?
3. Why are ports necessary in networking, and what happens if two applications try to use the same port on a single machine?
4. Explain the difference between public and private IP addresses, and why a server typically needs a public IP while a client does not.
5. Using the analogy of mailing a book, describe how TCP ensures that data sent over a network is reassembled correctly at the destination.
