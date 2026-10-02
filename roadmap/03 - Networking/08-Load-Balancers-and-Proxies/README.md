# Module 08: Load Balancers, Proxies, and Traffic Control

Welcome to **Module 08: Load Balancers and Proxies**. Modern distributed microservices, Kubernetes clusters, and cloud-native applications rely heavily on load balancing for horizontal scalability, zero-downtime deployments, and high availability.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deeply understand the difference between **Layer 4** (Transport/TCP/UDP) and **Layer 7** (Application/HTTP/gRPC) load balancing.
2. Differentiate between **Reverse Proxies**, **Forward Proxies**, and **API Gateways**.
3. Master load balancing algorithms: Round Robin, Weighted Least Connections, IP Hashing, and **Consistent Hashing**.
4. Configure production-ready **Health Checks**, prevent flap cascades, and execute zero-downtime **Graceful Connection Draining**.
5. Implement TLS strategies: **TLS Termination**, **TLS Passthrough (SNI)**, and **Mutual TLS (mTLS)**.
6. Diagnose common proxy errors: `502 Bad Gateway`, `504 Gateway Timeout`, and `499 Client Closed Request`.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Layer 4 vs. Layer 7 Load Balancing](./01-Layer-4-vs-Layer-7-Load-Balancing.md) | Network/Transport level (IPVS, AWS NLB) vs Application level (ALB, NGINX, Envoy) |
| 02 | [Proxies & Gateways Architecture](./02-Reverse-Proxies-Forward-Proxies-and-Gateways.md) | Forward proxy, reverse proxy, API gateways (Kong, Traefik, APISIX) |
| 03 | [Load Balancing Algorithms Deep Dive](./03-Load-Balancing-Algorithms-Deep-Dive.md) | Round Robin, Weighted, Least Connections, Consistent Hashing (Ketama) |
| 04 | [Health Checks & Graceful Draining](./04-Health-Checks-Flapping-and-Graceful-Drain.md) | Active vs passive probes, health thresholds, connection draining lifecycle |
| 05 | [TLS Termination, Passthrough & mTLS](./05-TLS-SSL-Termination-Passthrough-and-mTLS.md) | Offloading SSL, SNI-based passthrough, zero-trust mTLS in service meshes |
| 06 | [Session Persistence & Sticky Sessions](./06-Session-Persistence-and-Sticky-Sessions.md) | Cookie-based stickiness, problems with autoscaling, stateless session tokens |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Thundering herd collapse, connection draining failures during blue/green deploy |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Diagnosing 502/503/504 errors, epoll socket limits, TIME_WAIT connection pools |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Configuring an NGINX load balancer with upstream health checks and SSL |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Algorithm decision trees, NGINX upstream snippets, status codes cheat sheet |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Firewalls & iptables](../07-Firewalls-iptables-and-UFW/README.md) | [Networking Index](../README.md) | [01 - L4 vs L7](./01-Layer-4-vs-Layer-7-Load-Balancing.md) |
