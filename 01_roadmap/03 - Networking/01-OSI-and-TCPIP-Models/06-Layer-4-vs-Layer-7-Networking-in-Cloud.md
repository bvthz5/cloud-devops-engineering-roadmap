# 06 — Layer 4 vs Layer 7 in Cloud Infrastructure

Modern cloud architectures heavily differentiate between Layer 4 and Layer 7 load balancers and proxies.

---

## 1. Architectural Comparison

| Characteristic | Layer 4 (Transport / NLB) | Layer 7 (Application / ALB, Envoy) |
| :--- | :--- | :--- |
| **Inspection Depth** | IP addresses & Port numbers only | Full HTTP/HTTPS headers, URLs, cookies, body |
| **Routing Decisions** | Routes by IP:Port hash (Round Robin, 5-tuple)| Path routing (`/api` vs `/static`), host header (`app.com`)|
| **TLS Handling** | Pass-through (Client connects directly to backend)| SSL/TLS Termination & Re-encryption |
| **Latency & Overhead**| **Ultra-low latency (Microseconds)** | Higher latency (Milliseconds) |
| **Throughput** | Millions of packets/sec (Kernel bypass, IPVS)| High, but bounded by CPU parsing HTTP |
| **Cloud Service** | **AWS NLB**, Azure Load Balancer | **AWS ALB**, Google Cloud HTTPS LB, NGINX |

---

## 2. When to Choose L4 vs L7

- **Choose Layer 4 (NLB):** High-throughput databases (PostgreSQL, Redis), gaming servers, WebSockets without HTTP inspection, or when TLS client certificates must pass directly to backend pods.
- **Choose Layer 7 (ALB):** Microservices routing based on paths (`/users` -> Users Pod, `/orders` -> Orders Pod), HTTP/2 and gRPC load balancing, and WAF (Web Application Firewall) inspection.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Layer 3 Network IP Routing and Routers](./05-Layer-3-Network-IP-Routing-and-Routers.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
