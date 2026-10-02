# 09 - Load Balancers & Proxies: Interview Questions & Answers

### Q1: When would you choose an AWS NLB (Layer 4) over an AWS ALB (Layer 7)?
**Answer:** Choose NLB when you require:
1. Extreme throughput handling millions of requests per second with sub-millisecond latency.
2. Static or Elastic IP support directly on the load balancer endpoints.
3. Non-HTTP protocols (pure TCP, UDP, TLS passthrough, MQTT, gaming servers).
4. Direct Server Return (DSR) or preserving client source IP without `X-Forwarded-For` injection.

### Q2: What is the purpose of the `X-Forwarded-For` header?
**Answer:** Because a Layer 7 reverse proxy terminates the TCP connection and establishes a new one to the backend, the backend sees the proxy's IP as the client. The proxy appends the original client IP to the `X-Forwarded-For: <client>, <proxy1>, <proxy2>` header.

### Q3: What is the Ketama consistent hashing algorithm, and why is it used?
**Answer:** It maps both servers and request keys onto a continuous circular hash ring. When a server fails or is added, only a minimal fraction ($1/N$) of keys are remapped, preventing cache stampedes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
