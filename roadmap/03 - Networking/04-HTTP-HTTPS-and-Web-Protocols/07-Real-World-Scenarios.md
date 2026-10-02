# 07 — Real-World HTTP Production Scenarios

---

## Scenario 1: Diagnosing 502 Bad Gateway vs 504 Gateway Timeout

### 502 Bad Gateway:
- **What happened:** The reverse proxy reached out to the backend container, but the backend **refused connection** or crashed immediately (`Connection refused` on port 8080).
- **Fix:** Check if backend pod is running (`kubectl get pods`), verify port binding in container.

### 504 Gateway Timeout:
- **What happened:** The reverse proxy connected to the backend, but the backend application took longer than the proxy's `proxy_read_timeout` (e.g. 60s) to return response headers.
- **Fix:** Check for slow database queries, deadlocks, or increase proxy timeout.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - CORS & Security Headers](./06-CORS-and-Web-Security-Headers.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
