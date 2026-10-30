# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Upstream Latency Tracing in Access Logs

Inject upstream timing variables into Nginx access logs to diagnose whether latency originated in Nginx or the backend application:

```nginx
log_format upstream_timing '$remote_addr - [$time_local] "$request" '
                           'status=$status '
                           'req_time=$request_time '
                           'upstream_addr=$upstream_addr '
                           'upstream_connect_time=$upstream_connect_time '
                           'upstream_header_time=$upstream_header_time '
                           'upstream_response_time=$upstream_response_time';
```

### Metric Definitions:
- **`$upstream_connect_time`**: Time taken to establish TCP/TLS connection with backend. (High value = network latency or backend connection backlog).
- **`$upstream_header_time`**: Time until backend returns first header byte. (High value = slow database query or backend compute freeze).
- **`$upstream_response_time`**: Total time taken to receive entire response body from backend.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
