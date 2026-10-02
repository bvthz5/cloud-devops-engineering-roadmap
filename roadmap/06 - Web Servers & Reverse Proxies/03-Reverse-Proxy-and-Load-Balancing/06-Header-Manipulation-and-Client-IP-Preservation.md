# 06 - Header Manipulation and Client IP Preservation

## 1. The Lost Client IP Problem

When a reverse proxy terminates an incoming connection, the TCP socket connection to the backend originates from the **proxy's IP address**. The backend application sees the reverse proxy IP as the client, breaking rate limiting, GeoIP, and audit logs.

```text
[ Client: 203.0.113.195 ] ──► [ Reverse Proxy: 10.0.0.1 ] ──► [ Backend App ]
                                                               (Sees Remote IP = 10.0.0.1!)
```

---

## 2. Forwarding Client Identity Headers

```nginx
location / {
    proxy_pass http://backend_pool;

    # Preserves the original Host header requested by client
    proxy_set_header Host $host;

    # Passes the immediate client IP address
    proxy_set_header X-Real-IP $remote_addr;

    # Appends client IP to existing chain of proxy IPs
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    # Passes original protocol (http vs https)
    proxy_set_header X-Forwarded-Proto $scheme;

    # Standard RFC 7239 Forwarded header
    proxy_set_header Forwarded "for=$remote_addr;proto=$scheme";
}
```

---

## 3. PROXY Protocol (v1 and v2)

For Layer 4 TCP proxying where HTTP headers cannot be injected, the **PROXY Protocol** prepends a 1-line ASCII string (v1) or binary header (v2) containing the original client IP/Port to the TCP stream upon connection establishment.

```nginx
# Receiving PROXY Protocol from an AWS NLB or HAProxy
server {
    listen 80 proxy_protocol;
    listen 443 ssl proxy_protocol;

    # Replace $remote_addr with the IP extracted from PROXY protocol header
    set_real_ip_from 10.0.0.0/16; # Trusted proxy CIDR
    real_ip_header proxy_protocol;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Active vs Passive Health Checking](./05-Active-vs-Passive-Health-Checking.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
