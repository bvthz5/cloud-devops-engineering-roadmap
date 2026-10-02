# 05 - Dynamic JSON API and Zero-Downtime Config

## 1. The Caddy Administration REST API (`:2019`)

Caddy includes an administrative HTTP API listening locally on port `2019`. Through this API, operators can read, replace, or surgically patch individual parts of the running configuration using JSON pointers:

```bash
# 1. Export active configuration as JSON
curl http://localhost:2019/config/ | jq .

# 2. Add an upstream server dynamically with ZERO reloads
curl -X POST http://localhost:2019/config/apps/http/servers/srv0/routes/0/handle/0/routes/0/handle/0/upstreams \
  -H "Content-Type: application/json" \
  -d '{"dial": "10.0.1.15:8080"}'

# 3. Reload configuration from a Caddyfile via API
curl -X POST "http://localhost:2019/load" \
  -H "Content-Type: text/caddyfile" \
  --data-binary @Caddyfile
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Caddyfile Syntax & Directives](./04-Caddyfile-Syntax-Directives-and-Snippets.md) | [README](./README.md) | [06 - Caddy Ingress & Edge Proxy](./06-Caddy-as-a-Kubernetes-Ingress-and-Container-Edge.md) |
