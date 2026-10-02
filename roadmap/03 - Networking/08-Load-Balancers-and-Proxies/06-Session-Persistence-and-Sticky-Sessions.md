# 06 - Session Persistence and Sticky Sessions

## 1. What are Sticky Sessions?
Sticky sessions (session affinity) ensure that all requests from a specific user session are routed to the exact same backend server instance.

### Mechanisms:
- **Application-Generated Cookies:** Load balancer inspects an existing cookie (e.g. `JSESSIONID`).
- **Load Balancer-Generated Cookies:** Load balancer injects its own encrypted cookie (e.g., `AWSALB`).

---

## 2. The Cloud-Native Antipattern
Sticky sessions violate the 12-factor cloud-native principle of stateless services:
- **Autoscaling Breakdown:** When nodes scale down, active user sessions bound to that node are dropped.
- **Traffic Imbalance:** A single heavy user (or corporate office behind one NAT) can overload one server while others remain idle.

### Recommended Alternative: Centralized Shared State
Store session state externally in **Redis Cluster**, **Memcached**, or utilize stateless **JWT tokens**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - TLS Termination](./05-TLS-SSL-Termination-Passthrough-and-mTLS.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
