# 01 - Layer 4 vs. Layer 7 Load Balancing

## 1. Architectural Comparison

```
              LAYER 4 (Transport / NLB)               LAYER 7 (Application / ALB / Envoy)
              ========================               ===================================
Client                  Client                                 Client
  │                       │                                      │
  ▼                       ▼                                      ▼
[SYN] ───────────────► Forwarded directly               [SYN/ACK Complete Handshake]
                      to Backend                                 │
                          │                             Terminates TCP session.
                   Client IP preserved                  Inspects HTTP headers, path, cookies.
                   Ultra-low latency (µs)                        │
                   No TLS decryption                    Establishes NEW TCP session to Backend.
                   Millions of reqs/sec                          │
                                                        Feature-rich routing (/api vs /static)
```

| Feature | Layer 4 Load Balancing (L4) | Layer 7 Load Balancing (L7) |
|---|---|---|
| **OSI Layer** | Layer 4 (TCP, UDP, SCTP) | Layer 7 (HTTP/1.1, HTTP/2, HTTP/3, gRPC, WebSocket) |
| **Data Inspected** | Source IP, Dest IP, Source Port, Dest Port | Headers, Cookies, URL Paths, HTTP Methods, Query Params |
| **TCP Termination** | Pass-through or Direct Server Return (DSR) | Proxy terminates TCP connection and opens new TCP connection to backend |
| **TLS/SSL Decryption** | Typically passthrough (blind stream) | Fully decrypts TLS to inspect payloads and headers |
| **Performance & Latency** | Extreme throughput, microsecond latency | Higher CPU usage, slight latency overhead (milliseconds) |
| **Routing Capability** | IP/Port only | Path-based (`/api/*`), Host-based (`admin.example.com`), Header-based |
| **Popular Examples** | Linux IPVS, AWS NLB, HAProxy (mode tcp), Katran | NGINX, Envoy Proxy, AWS ALB, Traefik, HAProxy (mode http) |

---

## 2. Direct Server Return (DSR) at Layer 4

In ultra-scale Layer 4 load balancing (used by Google Maglev, Cloudflare Unimog, Facebook Katran):
- Request inbound traffic is small (e.g. GET request: 1 KB) and passes through the L4 load balancer.
- Response outbound traffic is huge (e.g. Video stream, HTML payload: 10 MB).
- **DSR allows backend servers to reply directly to the client's public IP**, completely bypassing the load balancer on the return path! This eliminates load balancer bandwidth bottlenecks.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Proxies & Gateways](./02-Reverse-Proxies-Forward-Proxies-and-Gateways.md) |
