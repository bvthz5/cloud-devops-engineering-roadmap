# Module 13: CDN, Edge Networks, and Anycast Routing

Welcome to **Module 13: CDN, Edge Networks, and Anycast Routing**. High-traffic global platforms achieve single-digit millisecond latency and withstand multi-terabit DDoS attacks through Content Delivery Networks (CDNs) and BGP Anycast routing.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deconstruct **BGP Anycast Routing** and understand how hundreds of edge datacenters advertise identical IP addresses globally.
2. Architect CDN topologies using **Points of Presence (PoPs)**, **Edge Caching**, and **Origin Shields**.
3. Master HTTP caching directives: `s-maxage`, `stale-while-revalidate`, and instantaneous cache purging strategies.
4. Execute code at the perimeter using **Edge Compute** (Cloudflare Workers, AWS Lambda@Edge).
5. Absorb massive volumetric **DDoS attacks** at the edge using BGP Anycast and Web Application Firewalls (WAF).
6. Accelerate dynamic API traffic through edge TCP termination and connection pooling.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [BGP Anycast Routing Architecture](./01-Anycast-Routing-Architecture-and-BGP.md) | Unicast vs Anycast, how internet traffic routes to the nearest edge PoP |
| 02 | [CDN Architecture & Edge Caching](./02-CDN-Architecture-PoPs-and-Edge-Caching.md) | PoP layout, cache tiers, origin shield, optimizing Cache Hit Ratio (CHR) |
| 03 | [Cache-Control & Invalidation](./03-Cache-Control-Headers-and-Invalidation-Strategies.md) | `s-maxage`, `stale-while-revalidate`, cache purge by surrogate keys and tags |
| 04 | [Edge Compute & Serverless](./04-Edge-Compute-Cloudflare-Workers-Lambda-Edge-Fastly-Compute.md) | V8 Isolates vs containers, perimeter authentication, header rewriting |
| 05 | [DDoS Mitigation & Edge WAF](./05-DDoS-Mitigation-Rate-Limiting-and-WAF-at-the-Edge.md) | Absorbing Tbps SYN floods, BGP Flowspec, OWASP top 10 WAF rules |
| 06 | [Dynamic Content Acceleration](./06-Dynamic-Content-Acceleration-and-TCP-Optimization.md) | Edge TCP termination, TLS offloading, long-haul private backbone routing |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | The Fastly global edge outage, caching bug leaking private session cookies |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Inspecting `CF-Cache-Status` / `X-Cache`, Anycast BGP route flapping |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Simulating CDN caching rules and testing `stale-while-revalidate` with curl |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Cache header decision matrix, CDN status codes, Anycast cheat sheet |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Overlay Networks & CNI](../12-Overlay-Networks-VXLAN-and-Container-CNI/README.md) | [Networking Index](../README.md) | [01 - Anycast Architecture](./01-Anycast-Routing-Architecture-and-BGP.md) |
