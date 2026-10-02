# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Enabling `DEBUG` Logging

When a container route is not appearing in Traefik, enable debug logs to view provider discovery events:

```yaml
# traefik.yml
log:
  level: DEBUG
```

Look for:
- `Skipping container: ...`: Container is missing `traefik.enable=true`.
- `Cannot find port for container: ...`: Container exposes multiple ports; specify `traefik.http.services.<name>.loadbalancer.server.port=<port>`.

---

## 2. Inspecting the Web Dashboard and API

```bash
# Query active routers via Traefik REST API
curl -s http://127.0.0.1:8080/api/http/routers | jq .
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
