# 03 — HTTPS, TLS Termination, and Certificates

HTTPS encrypts HTTP traffic by wrapping it inside Transport Layer Security (TLS).

---

## 1. TLS Termination vs End-to-End TLS

```text
[ Architecture A: TLS Termination at Edge (Standard) ]
Client ═════( HTTPS: Port 443 )═════► [ Load Balancer (ALB / Cloudflare) ] ─────( HTTP: Port 80 )─────► [ Backend Pod ]
                                       - Decrypts TLS
                                       - Offloads CPU overhead from pods
                                       - Injects X-Forwarded-For & X-Forwarded-Proto

[ Architecture B: End-to-End TLS (Zero Trust / PCI-DSS) ]
Client ═════( HTTPS: Port 443 )═════► [ Load Balancer ] ═════( HTTPS: Port 8443 )═════► [ Backend Pod / mTLS ]
                                       - Re-encrypts or passes through
```

---

## 2. Server Name Indication (SNI)

In modern cloud environments, a single IP address on an Application Load Balancer or NGINX server hosts hundreds of different domains.
Because TLS handshake occurs **before** HTTP headers are sent, the server wouldn't know which SSL certificate to present!
**SNI** solves this by having the client include the target hostname in plaintext inside the initial `ClientHello` packet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - HTTP Request and Response Anatomy](./02-HTTP-Request-and-Response-Anatomy.md) | [Index](../../../README.md) | [04 - HTTP Caching and Conditional Requests →](./04-HTTP-Caching-and-Conditional-Requests.md) |
