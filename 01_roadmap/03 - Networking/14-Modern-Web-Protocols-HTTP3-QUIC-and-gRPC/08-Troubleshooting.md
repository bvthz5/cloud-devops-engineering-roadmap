# 08 - Modern Protocols: Troubleshooting Guide

## 1. HTTP/3 Corporate Firewall Blockage

### The Problem
Clients successfully negotiate HTTP/3 over public consumer broadband, but corporate enterprise users fail to connect or experience fallback delays.

### Root Cause
Many traditional corporate firewalls strictly allow TCP port 443 for web traffic and block outbound **UDP port 443** by default.

### Best Practice: The `Alt-Svc` Header
Servers must never force HTTP/3 exclusively. Serve initial responses over HTTP/2 with the advertisement:
```http
Alt-Svc: h3=":443"; ma=86400
```
If the client cannot reach UDP port 443, it smoothly continues communicating over HTTP/2 without failing!

---

## 2. Common gRPC Status Code Triage

| Status Code | Meaning | Root Cause |
|---|---|---|
| `UNAVAILABLE` (14) | Cannot connect to remote service | Service crashed, pod terminating, or load balancer has zero healthy backends |
| `DEADLINE_EXCEEDED` (4) | Request exceeded client timeout | Downstream dependency database deadlock or timeout setting too aggressive |
| `FAILED_PRECONDITION` (9) | System state rejected call | Calling refund on an unpaid order |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
