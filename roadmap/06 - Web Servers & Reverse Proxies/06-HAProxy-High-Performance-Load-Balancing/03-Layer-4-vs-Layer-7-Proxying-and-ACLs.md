# 03 - Layer 4 vs Layer 7 Proxying and ACLs

## 1. `mode tcp` (Layer 4) vs `mode http` (Layer 7)

```text
Layer 4 TCP (mode tcp):
- HAProxy acts as a high-speed raw TCP/SSL tunnel.
- Used for PostgreSQL, MySQL, Redis, RabbitMQ, and TLS Passthrough.
- Cannot inspect HTTP headers, URL paths, or cookies.

Layer 7 HTTP (mode http):
- Full HTTP parsing engine.
- Inspects headers, injects X-Forwarded-For, performs path-based routing.
- Compresses responses, manages HTTP/2, and validates WebSocket handshakes.
```

---

## 2. Advanced Access Control Lists (ACLs)

HAProxy features an expressive, blazing-fast ACL evaluation language:

```haproxy
frontend main_ingress
    bind *:443 ssl crt /etc/haproxy/certs/site.pem
    mode http

    # 1. IP-based ACLs
    acl is_internal_network src 10.0.0.0/8 192.168.0.0/16
    acl is_blocked_country  src -f /etc/haproxy/blocked_ips.lst

    # 2. Path & Header-based ACLs
    acl is_websocket hdr(Upgrade) -i WebSocket
    acl is_admin_uri path_beg -i /admin /dashboard
    acl is_post_method method POST

    # Deny access to admin panel from public IPs
    http-request deny if is_admin_uri !is_internal_network
    http-request deny if is_blocked_country

    # Route WebSockets to dedicated stateful backend
    use_backend ws_cluster if is_websocket
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Configuration Structure](./02-Configuration-Structure-Global-Defaults-Frontend-Backend.md) | [README](./README.md) | [04 - Stick-Tables & Advanced Rate Limiting](./04-Stick-Tables-and-Advanced-Rate-Limiting.md) |
