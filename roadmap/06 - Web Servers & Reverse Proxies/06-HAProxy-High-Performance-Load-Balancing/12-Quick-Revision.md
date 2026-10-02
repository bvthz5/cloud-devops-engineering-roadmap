# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Validate HAProxy configuration syntax
haproxy -c -f /etc/haproxy/haproxy.cfg

# Query stats via socket
echo "show info" | socat stdio /run/haproxy/admin.sock
echo "show stat" | socat stdio /run/haproxy/admin.sock

# Drain server dynamically
echo "set server backend_pool/srv1 state drain" | socat stdio /run/haproxy/admin.sock
```

```haproxy
# Minimal haproxy.cfg Template
global
    stats socket /run/haproxy/admin.sock mode 660 level admin
    daemon

defaults
    mode http
    timeout connect 5s
    timeout client 50s
    timeout server 50s

frontend http_in
    bind *:80
    default_backend servers

backend servers
    balance roundrobin
    option httpchk GET /health
    server s1 10.0.1.10:8080 check
    server s2 10.0.1.11:8080 check
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [07 - Traefik Cloud-Native Reverse Proxy](../07-Traefik-Cloud-Native-Reverse-Proxy/README.md) |
