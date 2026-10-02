# 08 - Troubleshooting & Diagnostic Runbooks

## 1. The Envoy Admin Interface (`:15000`)

Envoy exposes an administrative HTTP server (default port `15000`) providing live inspection of internal proxy state:

```bash
# 1. Dump entire currently loaded active configuration (LDS, RDS, CDS, EDS)
curl -s http://127.0.0.1:15000/config_dump | jq .

# 2. View membership and health status of all upstream clusters
curl -s http://127.0.0.1:15000/clusters

# 3. Dynamically change logging verbosity to DEBUG without restarting
curl -X POST http://127.0.0.1:15000/logging?level=debug

# 4. View active circuit breaker status
curl -s http://127.0.0.1:15000/stats | grep "circuit_breakers"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
