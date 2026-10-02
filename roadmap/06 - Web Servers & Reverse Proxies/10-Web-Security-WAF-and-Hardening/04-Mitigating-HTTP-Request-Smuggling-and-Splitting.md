# 04 - Mitigating HTTP Request Smuggling and Splitting

## 1. HTTP Request Smuggling Mechanics (CL.TE / TE.CL)

HTTP Request Smuggling occurs when a front-end reverse proxy and a back-end server disagree on the boundary of an HTTP request:
- **`Content-Length` (CL)** specifies the request body size in bytes.
- **`Transfer-Encoding: chunked` (TE)** specifies that data arrives in chunked hex lengths.

If both headers are present, RFC 7230 states `Transfer-Encoding` must take precedence. However, if the front-end uses `Content-Length` and the back-end uses `Transfer-Encoding`, an attacker can "smuggle" an arbitrary second request inside the first body, hijacking subsequent user sessions!

```text
Front-end Proxy (uses Content-Length: 13):
[ POST / HTTP/1.1 \r\n Transfer-Encoding: chunked \r\n Content-Length: 13 \r\n\r\n 0 \r\n\r\n GPOST / ] ──► (Forwards 13 bytes)
                                                                                       │
Backend Server (uses Transfer-Encoding):                                               ▼
[ Reads up to '0' (End of chunk) ] ──► [ Leaves "GPOST /" unread in TCP buffer! ]
                                                     │
Subsequent Legitimate User Request Arrives:          ▼
[ GET /index.html HTTP/1.1 ] ──► Prefixed by buffer! Becomes: [ GPOST /index.html ] ──► Hijacked!
```

---

## 2. Hardening Reverse Proxies Against Smuggling

```nginx
# Enforce strict RFC compliance in Nginx
http {
    # Reject malformed requests with underscores or invalid characters in headers
    ignore_invalid_headers on;
    underscores_in_headers off;

    # Always enforce HTTP/1.1 with clean connection headers to upstream
    proxy_http_version 1.1;
    proxy_set_header Connection "";
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - HTTP Security Headers](./03-HTTP-Security-Headers-CSP-HSTS-Permissions-Policy.md) | [README](./README.md) | [05 - DDoS Mitigation & Slowloris](./05-DDoS-Mitigation-Slowloris-and-Flood-Protection.md) |
