# 12 - Quick-Revision & Enterprise Cheat Sheet

```yaml
# Docker Label Cheat Sheet
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.app.entrypoints=websecure"
  - "traefik.http.routers.app.rule=Host(`app.example.com`)"
  - "traefik.http.routers.app.tls.certresolver=letsencrypt"
  - "traefik.http.services.app.loadbalancer.server.port=8080"
  - "traefik.http.routers.app.middlewares=my-stripprefix"
```

```bash
# Verify permissions on acme.json
chmod 600 /etc/traefik/acme.json

# Check Traefik routers via API
curl http://localhost:8080/api/http/routers
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Envoy-Proxy-and-Service-Mesh-Data-Plane) →](../08-Envoy-Proxy-and-Service-Mesh-Data-Plane/01-Envoy-Architecture-Threading-and-Event-Model.md) |
