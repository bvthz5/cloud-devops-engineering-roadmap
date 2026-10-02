# 1. Web Server Fundamentals: HTTP/HTTPS, DNS & Ports

## Client-Server Architecture

A web server is a software process that listens on network ports, parses incoming HTTP/HTTPS requests from clients (browsers, mobile apps, API clients), and returns appropriate HTTP responses (HTML, JSON, static assets) or proxies requests to upstream application servers.

```text
[ Client / Browser ] 
        │
        ├───────── 1. DNS Lookup (example.com → 192.0.2.1)
        │
        ├───────── 2. TCP Handshake (SYN → SYN-ACK → ACK) [Port 80/443]
        │
        ├───────── 3. TLS Handshake (Client Hello ↔ Server Hello) [Port 443]
        │
        ▼
[ Web Server (Nginx / Apache / Caddy) ] ── (Static file OR Proxy to App)
        │
        ▼
[ HTTP Response (Status Code + Headers + Body) ]
```

## HTTP Protocol Evolution

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
| --- | --- | --- | --- |
| **Transport Protocol** | TCP | TCP | UDP (QUIC) |
| **Connection Model** | Head-of-Line Blocking per domain | Binary Multiplexing over single TCP connection | Independent streams over UDP (No HOL blocking) |
| **Header Compression** | Plaintext (uncompressed) | HPACK binary compression | QPACK binary compression |
| **TLS Requirement** | Optional (HTTP/HTTPS) | Virtually mandatory in browsers | Mandatory (Built into QUIC) |

## Standard Network Ports

- **Port 80:** Standard unencrypted HTTP traffic.
- **Port 443:** Standard TLS/SSL encrypted HTTPS traffic.
- **Port 8080 / 8443:** Common alternative ports for backend apps or application servers (Tomcat, Node.js).
- **Port 9000:** Default FastCGI port (PHP-FPM).

## DNS Resolution Role
Before a web server can receive a request, the client resolves the domain name via DNS:
1. **A Record:** Maps domain to IPv4 address (`example.com → 192.0.2.1`).
2. **AAAA Record:** Maps domain to IPv6 address.
3. **CNAME Record:** Alias domain to another canonical domain name (`www.example.com → example.com`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Nginx Architecture and Process Model](./02-Nginx-Architecture-and-Process-Model.md) |
