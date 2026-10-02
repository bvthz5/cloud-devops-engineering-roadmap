# 03 - Reverse Proxy and Load Balancing

Reverse proxies and load balancers are the cornerstone of scalable, resilient cloud architectures. Sitting between external clients and backend server fleets, a reverse proxy terminates connections, balances traffic, offloads SSL/TLS encryption, enforces security policies, and isolates internal microservices from the public internet.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Forward Proxy vs Reverse Proxy Architecture](./01-Forward-Proxy-vs-Reverse-Proxy-Architecture.md) | Architectural distinctions, traffic direction, egress gateways, and security isolation. |
| 02 | [Layer 4 (TCP/UDP) vs Layer 7 (HTTP) Balancing](./02-Layer-4-vs-Layer-7-Load-Balancing.md) | Stream routing vs application routing, performance trade-offs, packet rewrites vs TLS termination. |
| 03 | [Load Balancing Algorithms](./03-Load-Balancing-Algorithms.md) | Round Robin, Least Connections, IP Hash, Consistent Hashing (Ketama ring), Weighted balancing. |
| 04 | [Upstream Management & Buffer Tuning](./04-Upstream-Management-and-Buffer-Tuning.md) | Nginx upstream blocks, keepalive pools, `proxy_buffers`, `proxy_buffer_size`, disk buffering. |
| 05 | [Active vs Passive Health Checking](./05-Active-vs-Passive-Health-Checking.md) | In-band failure detection (`max_fails`, `fail_timeout`) vs out-of-band proactive synthetic probes. |
| 06 | [Header Manipulation & Client IP Preservation](./06-Header-Manipulation-and-Client-IP-Preservation.md) | `X-Forwarded-For`, `X-Forwarded-Proto`, `X-Real-IP`, RFC 7239 `Forwarded`, PROXY protocol v1/v2. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Proxy buffer overflow disk trashing, upstream keepalive exhaustion. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging 502/504 errors, packet captures with `tcpdump`, tracing upstream latency headers. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior DevOps/SRE questions on load balancing math, consistent hashing, and connection pools. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Zero-downtime weighted upstream pool; Lab 2: PROXY protocol configuration; Lab 3: Buffer tuning. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for upstream directives, buffer formulas, and balancing algorithms. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Apache HTTP Server](../02-Apache-HTTP-Server/README.md) | [README](./README.md) | [01 - Forward vs Reverse Proxy](./01-Forward-Proxy-vs-Reverse-Proxy-Architecture.md) |
