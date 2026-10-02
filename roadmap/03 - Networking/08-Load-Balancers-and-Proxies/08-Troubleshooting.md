# 08 - Load Balancers & Proxies: Troubleshooting Guide

## 1. HTTP Status Code Diagnostic Matrix

| Status Code | Meaning | Immediate Root Cause |
|---|---|---|
| **502 Bad Gateway** | Proxy received an invalid response or TCP connection reset from backend | Backend crashed, port closed, backend backlog queue full |
| **503 Service Unavailable** | Load balancer has zero healthy backends available in the pool | All backends failed health checks or pool is administratively drained |
| **504 Gateway Timeout** | Proxy connected to backend, but backend did not reply before timeout | Backend database deadlock, unindexed query, backend thread starvation |
| **499 Client Closed Request** | NGINX-specific code: Client disconnected before NGINX sent response | Client browser timed out, network drop, user closed tab |

---

## 2. Ephemeral Port Exhaustion on Reverse Proxies

When a reverse proxy handles 50,000 requests/second to a small set of backend IPs, it can exhaust its available client outbound ports (`ip_local_port_range`):

```bash
# Check current ephemeral port range
sysctl net.ipv4.ip_local_port_range
net.ipv4.ip_local_port_range = 32768 60999  # Only ~28,000 available ports!

# Fix: Expand range and enable TIME_WAIT socket reuse
sudo sysctl -w net.ipv4.ip_local_port_range="10240 65535"
sudo sysctl -w net.ipv4.tcp_tw_reuse=1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
