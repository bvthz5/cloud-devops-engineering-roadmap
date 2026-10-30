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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (09-Caddy-and-Modern-HTTP3-Web-Servers) →](../09-Caddy-and-Modern-HTTP3-Web-Servers/01-Caddy-Architecture-and-The-Go-Runtime.md) |
