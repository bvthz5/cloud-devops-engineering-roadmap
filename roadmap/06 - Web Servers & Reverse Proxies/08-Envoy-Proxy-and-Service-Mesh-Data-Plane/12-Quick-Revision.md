# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Envoy Admin CLI Shortcuts
curl http://localhost:15000/stats              # Dump all metrics
curl http://localhost:15000/clusters           # View upstream server status
curl http://localhost:15000/config_dump        # View active in-memory xDS config
curl -X POST http://localhost:15000/logging?level=debug # Enable debug logs
```

```yaml
# Core Envoy xDS Concept Map:
# LDS -> Listeners (Port, TLS, HCM)
# RDS -> Routes (VirtualHost, Paths, Clusters)
# CDS -> Clusters (Service pools, LB policy, Circuit Breakers)
# EDS -> Endpoints (Live IP:Port members)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [09 - Caddy & Modern HTTP/3 Web Servers](../09-Caddy-and-Modern-HTTP3-Web-Servers/README.md) |
