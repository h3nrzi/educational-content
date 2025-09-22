# Domain Name System (DNS)

## Overview

The **Domain Name System (DNS)** is a decentralized system that translates human-friendly domain names (e.g., `google.com`) into IP addresses (e.g., `172.217.0.1`) required for network communication. It acts like a phone book, mapping names to addresses, enabling users to access servers without memorizing IP addresses. For system design interviews, understanding the high-level functionality and key components of DNS is sufficient, though some additional context helps in grasping its role in large-scale systems.

## Key Concepts

### 1. Purpose of DNS

- **Problem**: Users want to access servers using easy-to-remember domain names (e.g., `google.com`) rather than numerical IP addresses.
- **Solution**: DNS resolves domain names to IP addresses, allowing clients to send requests to the correct server.
- **Process**:
  - Client (e.g., browser) queries DNS for the IP address of a domain (e.g., `google.com`).
  - DNS returns the IP address.
  - Client uses the IP address to send a request to the server and receive a response.
- **Example**: Typing `google.com` in a browser or using `nslookup google.com` in a terminal retrieves the IP address (e.g., `172.217.0.1`), which directs the request to Google’s server.

### 2. DNS Components

- **Internet Corporation for Assigned Names and Numbers (ICANN)**:
  - Non-profit organization that manages global domain names and IP address allocations.
  - Ensures only one entity owns a domain (e.g., `google.com`).
- **Domain Registrars**:
  - Accredited resellers (e.g., Google Domains, Namecheap, GoDaddy) that sell domains on behalf of ICANN.
  - Maintain **DNS records** mapping domains to IP addresses.
- **DNS Records**:
  - Store information for resolving domain names.
  - **A Record (Address Record)**: Maps a domain (e.g., `neatcode.io`) to an IP address.
  - Other record types exist (e.g., CNAME, MX), but A records are most relevant for basic resolution.
- **Internet Service Provider (ISP)**:
  - Facilitates DNS queries by routing requests through DNS servers.
  - Interacts with ICANN servers and registrars to resolve domain names.

### 3. DNS Resolution Process (Simplified)

- **Steps**:
  1. Client sends a DNS query for a domain (e.g., `google.com`) via the ISP.
  2. ISP routes the query through a chain of DNS servers, including:
     - **Root servers** (managed by ICANN).
     - **Top-level domain (TLD) servers** (e.g., for `.com`, `.io`).
     - **Registrar servers** (e.g., Google Domains for `neatcode.io`).
  3. The registrar’s server returns the IP address from the DNS A record.
  4. Client caches the IP address locally to avoid repeated queries.
  5. Client uses the IP address to communicate with the server.
- **Caching**: IP addresses are cached on the client’s device (e.g., disk) to reduce future DNS queries, improving performance.

### 4. Domain and URL Components

- **URL Structure**: `protocol://subdomain.domain.tld/path?query_params`
  - **Protocol**: Typically `http` (port 80) or `https` (port 443); others like `file://` for local access.
  - **Top-Level Domain (TLD)**: `.com`, `.io`, `.jp` (country codes), managed by ICANN.
  - **Domain Name**: Core name (e.g., `google` in `google.com`), purchased via registrars.
  - **Subdomain**: Optional prefix (e.g., `www`, `domains` in `domains.google.com`). Owners control subdomains.
  - **Path**: Specific resource on the server (e.g., `/search` in `google.com/search`).
  - **Query Parameters**: Additional data (e.g., `?q=test` for search queries).
- **Example**:
  - Owning `google.com` allows creating subdomains like `www.google.com` or `domains.google.com`.
  - DNS A record maps `google.com` to an IP address for server access.

### 5. Additional Notes

- **Static vs. Dynamic IPs**:
  - Servers typically use **static IPs** for consistent access.
  - **Dynamic DNS** allows domains to map to changing IPs, though less common for public servers.
- **Server Definition**:
  - A computer with a **public IP address**, configured firewalls to allow external traffic, and running services to handle requests (e.g., web server on port 80/443).
  - Differs from client devices, which often use private IPs behind routers and block external traffic.
- **Complexity**:
  - DNS is a distributed system involving multiple servers (root, TLD, registrar).
  - For system design, focus on its role as a lookup service, not intricate details.

## Key Takeaways

- **DNS Role**: Translates domain names to IP addresses, enabling user-friendly server access.
- **Decentralized System**: Involves ICANN, registrars, ISPs, and DNS servers to resolve queries.
- **Caching**: Reduces latency by storing IP addresses locally.
- **System Design Relevance**: Understand DNS as a critical component for routing client requests to servers; no need for low-level details in interviews.

## Questions for Review

1. What problem does the Domain Name System (DNS) solve, and how does it function at a high level to enable client-server communication?
2. Explain the roles of ICANN and domain registrars in the DNS ecosystem.
3. What is an A record, and how does it contribute to resolving a domain name like `neatcode.io`?
4. Why is caching IP addresses on a client’s device important, and how does it impact performance in accessing websites?
5. Describe the components of a URL (e.g., `https://www.google.com/search?q=test`) and explain how owning a domain allows control over subdomains.
