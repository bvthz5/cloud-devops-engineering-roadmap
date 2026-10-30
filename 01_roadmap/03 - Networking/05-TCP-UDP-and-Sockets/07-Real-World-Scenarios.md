# 07 — Real-World TCP Production Scenarios

---

## Scenario 1: Ephemeral Port Exhaustion on NGINX Upstream

### Incident Summary
NGINX reverse proxy fails to connect to backend microservices, returning HTTP 502 with:
`connect() failed (99: Cannot assign requested address) while connecting to upstream`.

### Root Cause Analysis
Every time NGINX connects to the backend without HTTP keep-alive, it opens an ephemeral port. Sockets enter `TIME_WAIT` for 60 seconds. The 28,000 default ephemeral ports are quickly exhausted.

### Production Solution
1. Enable `keepalive` in NGINX `upstream` block.
2. Enable `net.ipv4.tcp_tw_reuse = 1` in sysctl.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - TCP Tuning and Optimization in Linux](./06-TCP-Tuning-and-Optimization-in-Linux.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
