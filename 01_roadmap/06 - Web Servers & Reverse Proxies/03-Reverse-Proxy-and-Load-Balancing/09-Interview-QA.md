# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between Layer 4 and Layer 7 load balancing?
**Answer**: Layer 4 load balancing operates at the Transport layer (TCP/UDP), routing packets based solely on IP and port without decrypting or inspecting the payload. It offers ultra-high throughput and low CPU overhead. Layer 7 balancing operates at the Application layer (HTTP/HTTPS/gRPC), terminating TLS and inspecting headers, cookies, and URI paths for smart routing at the expense of higher CPU processing.

### Q2: Why is `proxy_http_version 1.1; proxy_set_header Connection "";` mandatory for Nginx upstream keepalive?
**Answer**: By default, Nginx proxies requests to upstream backends using HTTP/1.0, which sets `Connection: close` on every request. Setting `proxy_http_version 1.1` and clearing the `Connection` header instructs the backend to keep the TCP connection open for reuse by the upstream keepalive pool.

### Q3: Explain Consistent Hashing and why it is superior to modulo hashing for distributed caching.
**Answer**: In modulo hashing (`hash(key) % N`), adding or removing a server changes the modulus, invalidating nearly 100% of cached keys across the cluster. In Consistent Hashing (Ketama ring), servers and keys map to a 360-degree ring. When a server is added or removed, only $1/N$ of keys are remapped, preserving cache hit ratios.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
