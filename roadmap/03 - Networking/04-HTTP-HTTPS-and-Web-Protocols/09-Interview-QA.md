# 09 — HTTP & Web Protocols Interview Q&A

10 technical interview questions for DevOps, SRE, and Cloud roles.

---

### Q1: What is the technical difference between HTTP 502 and HTTP 504?
**Answer:**
- **502 Bad Gateway:** The proxy contacted the upstream backend server, but received an invalid or terminated response (e.g. backend process crashed, port was closed, or connection was reset immediately).
- **504 Gateway Timeout:** The proxy successfully connected to the upstream backend, but the backend failed to return a complete HTTP response within the proxy's configured timeout window (e.g. slow database query, execution deadlock).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
