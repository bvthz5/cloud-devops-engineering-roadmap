# 07 — Real-World Production Scenarios: OSI & TCP/IP

---

## Scenario 1: The Misleading "Connection Timeout" (L4 vs L7)

### Incident Summary
A mobile app team reports that an internal API returns `504 Gateway Timeout`.
The developer insists: "The network is down! Packets aren't reaching the server."

### Root Cause Analysis
1. The SRE runs `curl -v http://alb.internal/api/orders`.
2. Notice: The TCP 3-way handshake with the ALB finishes in 2ms (**Layer 4 is 100% healthy!**).
3. The HTTP request is sent, but 60 seconds later, the ALB responds with HTTP 504.
4. Investigation reveals the backend container behind the ALB was stuck in a database deadlock. The ALB reached its `idle_timeout` waiting for the backend application (**Layer 7 issue**).

### Lesson
A `Connection Refused` is Layer 4. A `504 Gateway Timeout` means Layer 4 connected successfully, but Layer 7 timed out waiting for an application response.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Layer 4 vs Layer 7 Networking in Cloud](./06-Layer-4-vs-Layer-7-Networking-in-Cloud.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
