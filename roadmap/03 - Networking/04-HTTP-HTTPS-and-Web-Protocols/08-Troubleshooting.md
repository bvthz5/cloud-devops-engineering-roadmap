# 08 — HTTP Troubleshooting Guide & curl Runbook

---

## 1. Advanced Diagnostics with `curl`

```bash
# 1. Print response headers, status code, and TLS handshake
curl -Iv https://api.github.com

# 2. Simulate a browser CORS preflight request
curl -I -X OPTIONS https://api.company.com/users   -H "Origin: https://app.company.com"   -H "Access-Control-Request-Method: POST"

# 3. Test HTTP/2 support
curl -I --http2 https://www.google.com

# 4. Measure granular HTTP latency breakdown
curl -w "
DNS: %{time_namelookup}s
Connect: %{time_connect}s
TLS: %{time_appconnect}s
TTFB: %{time_starttransfer}s
Total: %{time_total}s
"   -o /dev/null -s https://example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
