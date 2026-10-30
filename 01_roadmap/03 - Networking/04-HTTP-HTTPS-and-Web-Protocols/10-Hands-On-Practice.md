# 10 — Hands-On Practice Labs: HTTP

---

## Lab 1: Profiling HTTP Latency Breakdown

```bash
# Profile TTFB (Time to First Byte) vs TLS handshake time
curl -w "DNS: %{time_namelookup}s | Connect: %{time_connect}s | TLS: %{time_appconnect}s | TTFB: %{time_starttransfer}s | Total: %{time_total}s
"   -o /dev/null -s https://httpbin.org/get
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
