# 12 - Load Balancers & Proxies: Quick Revision Cheat Sheet

## Architectural Cheat Sheet

| Feature | L4 Load Balancer | L7 Load Balancer |
|---|---|---|
| Level | TCP / UDP packet level | HTTP / gRPC application payload |
| Speed | Microsecond latency | Millisecond latency |
| TLS | Passthrough or offload | Full termination & inspection |
| Routing | Source/Dest IP & Port | Path `/api`, Hostname, Cookies |

## Error Diagnosis Cheat Sheet
- `502 Bad Gateway`: Backend dead, listening socket closed, or application crashed.
- `503 Service Unavailable`: All backends failed health checks.
- `504 Gateway Timeout`: Backend database locked or processing took longer than `proxy_read_timeout`.
- `499 Client Closed`: Client gave up waiting and terminated the connection.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (09-VPN-and-VPC-Networking) →](../09-VPN-and-VPC-Networking/01-VPN-Protocols-IPsec-WireGuard-and-OpenVPN.md) |
