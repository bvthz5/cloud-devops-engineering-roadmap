# 06 - HAProxy High-Performance Load Balancing

HAProxy (High Availability Proxy) is the industry benchmark for ultra-high-performance, rock-solid Layer 4 (TCP) and Layer 7 (HTTP) reverse proxying and load balancing. Trusted by GitHub, Stack Overflow, Reddit, and AWS, HAProxy processes millions of requests per second with microsecond latency, zero-copy packet processing, and state-of-the-art DDoS and traffic management capabilities.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [HAProxy Architecture & Event-Driven Engine](./01-HAProxy-Architecture-and-Event-Driven-Engine.md) | Single-process event loop, `nbthread` multi-threading, zero-copy TCP splicing, kernel bypass. |
| 02 | [Configuration Structure: Global, Defaults, Frontend & Backend](./02-Configuration-Structure-Global-Defaults-Frontend-Backend.md) | The 4 core sections, `listen` blocks, timeout tuning (`timeout client/server/connect`). |
| 03 | [Layer 4 vs Layer 7 Proxying & ACLs](./03-Layer-4-vs-Layer-7-Proxying-and-ACLs.md) | `mode tcp` vs `mode http`, powerful ACL matching primitives (`path_beg`, `hdr(host)`, `src`). |
| 04 | [Stick-Tables & Advanced Rate Limiting](./04-Stick-Tables-and-Advanced-Rate-Limiting.md) | High-speed in-memory stick-tables, sliding window request tracking, DDoS abuse defense. |
| 05 | [Active Health Checks & Graceful Failover](./05-Active-Health-Checks-and-Graceful-Failover.md) | Proactive HTTP/TCP health checks, `inter`, `rise`, `fall`, `backup` servers, graceful draining. |
| 06 | [Dynamic Reconfiguration: Runtime API & Data Plane API](./06-Dynamic-Reconfiguration-Runtime-API-and-Data-Plane-API.md) | Unix socket Runtime API, live server state modification without reloads, REST Data Plane API. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Master-worker reload packet loss, stick-table memory exhaustion. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Built-in Stats page, Unix socket inspection with `socat`, dissecting HAProxy timing logs (`%TR`, `%Tt`). |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on HAProxy vs Nginx, stick-tables, and TCP tuning. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Production Stats dashboard & ACL routing; Lab 2: DDoS stick-table limiter; Lab 3: Runtime API. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for haproxy.cfg syntax, ACL primitives, and socket CLI commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Caching & Rate Limiting](../05-Caching-and-Rate-Limiting/README.md) | [README](./README.md) | [01 - HAProxy Architecture](./01-HAProxy-Architecture-and-Event-Driven-Engine.md) |
