# 10 - Hands-On Practice Labs

## Lab 1: HAProxy Production Stats Dashboard and Rate Limiting

### Objective
Configure an HAProxy frontend with rate limiting via stick-tables and expose a protected stats web dashboard.

### Implementation
```haproxy
global
    stats socket /run/haproxy/admin.sock mode 660 level admin
    maxconn 10000

defaults
    mode http
    timeout connect 5s
    timeout client 30s
    timeout server 30s

# Protected Stats Dashboard
listen stats
    bind *:8404
    stats enable
    stats uri /
    stats refresh 5s
    stats auth admin:SuperSecretPass123!

# Production Web Ingress
frontend fe_ingress
    bind *:80
    
    # Rate Limiting: 50 requests per 10s per IP
    stick-table type ip size 100k expire 5m store http_req_rate(10s)
    http-request track-sc0 src
    http-request deny deny_status 429 if { sc_http_req_rate(0) gt 50 }

    default_backend be_nodes

backend be_nodes
    balance roundrobin
    option httpchk GET /health
    server node1 127.0.0.1:8001 check
    server node2 127.0.0.1:8002 check
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
