# 06 - Web Servers & Reverse Proxies

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

Web servers and reverse proxies sit directly at the edge of modern cloud, microservices, and enterprise infrastructure. A Platform Engineer, DevOps Engineer, or SRE must master everything from event-driven HTTP processing (Nginx), traditional process architectures (Apache), high-throughput load balancing (HAProxy), dynamic cloud-native service discovery (Traefik), service mesh data planes (Envoy), memory-safe HTTP/3 servers (Caddy), to deep Layer 7 Web Application Firewall (WAF) security and request smuggling defense.

---

## 📌 Complete Curriculum Modules

| # | Module | Core Focus & Engineering Scope | Status |
|---|---|---|:---:|
| 01 | [**01. Nginx Architecture & Configuration**](./01-Nginx-Architecture-and-Configuration/README.md) | Event-driven epoll loop, master-worker process model, context hierarchy, location matching priority (`=, ^~, ~, ~*, /`), zero-copy `sendfile`, and zero-downtime hot reloads. | ✅ Complete |
| 02 | [**02. Apache HTTP Server**](./02-Apache-HTTP-Server/README.md) | Multi-Processing Modules (`prefork`, `worker`, `event`), `.htaccess` performance implications, `mod_rewrite`, `mod_proxy_balancer`, and information disclosure hardening. | ✅ Complete |
| 03 | [**03. Reverse Proxy & Load Balancing**](./03-Reverse-Proxy-and-Load-Balancing/README.md) | Layer 4 vs Layer 7 balancing, algorithms (Least Conn, IP Hash, Consistent Hash Ketama ring), upstream keepalive connection pooling, proxy buffer tuning, and PROXY protocol. | ✅ Complete |
| 04 | [**04. SSL/TLS Certificates & HTTPS**](./04-SSL-TLS-Certificates-and-HTTPS/README.md) | TLS 1.2 vs 1.3 handshakes, Forward Secrecy (ECDHE), PKI certificate chains, automated ACME (Let's Encrypt/Certbot HTTP-01 & DNS-01), OCSP Stapling, and Zero-Trust mTLS. | ✅ Complete |
| 05 | [**05. Caching & Rate Limiting**](./05-Caching-and-Rate-Limiting/README.md) | HTTP Cache-Control, Nginx 1-second microcaching, cache stampede defense (`proxy_cache_use_stale updating`), Leaky Bucket rate limiting (`limit_req_zone`, `burst`, `nodelay`), and connection throttling. | ✅ Complete |
| 06 | [**06. HAProxy High-Performance Load Balancing**](./06-HAProxy-High-Performance-Load-Balancing/README.md) | Single-process event loop, `nbthread` multi-threading, zero-copy TCP splicing (`splice()`), stick-tables for sliding-window DDoS defense, active HTTP health probes, and Unix Runtime CLI socket. | ✅ Complete |
| 07 | [**07. Traefik Cloud-Native Reverse Proxy**](./07-Traefik-Cloud-Native-Reverse-Proxy/README.md) | Dynamic provider discovery (Docker daemon labels, Kubernetes API), EntryPoints, Routers, Middlewares, Services, IngressRoute CRD, automated ACME TLS, and OpenTelemetry tracing. | ✅ Complete |
| 08 | [**08. Envoy Proxy & Service Mesh Data Plane**](./08-Envoy-Proxy-and-Service-Mesh-Data-Plane/README.md) | Threading architecture, dynamic gRPC streaming xDS APIs (LDS, RDS, CDS, EDS), HTTP connection manager filter chains, circuit breaking, outlier detection ejection, and gRPC-Web bridging. | ✅ Complete |
| 09 | [**09. Caddy & Modern HTTP/3 Web Servers**](./09-Caddy-and-Modern-HTTP3-Web-Servers/README.md) | Go memory safety, automatic HTTPS with Let's Encrypt / ZeroSSL fallback, On-Demand TLS for SaaS, native HTTP/3 (QUIC) over UDP, Caddyfile matchers/snippets, and JSON REST admin API. | ✅ Complete |
| 10 | [**10. Web Security, WAF & Hardening**](./10-Web-Security-WAF-and-Hardening/README.md) | ModSecurity v3 / Coraza WAF with OWASP Core Rule Set (CRS v4) anomaly scoring, security headers (CSP, HSTS, X-Frame-Options), HTTP Request Smuggling (CL.TE / TE.CL) mitigation, and Slowloris defense. | ✅ Complete |

---

## 🛠️ Module Structure Standard

Every module in this domain strictly adheres to the enterprise 13-file roadmap standard:
1. `README.md` — Architectural overview & module syllabus
2. `01-` to `06-` — Deep technical guides with diagrams, code blocks, and internal mechanisms
3. `07-Real-World-Scenarios.md` — Real production post-mortems and multi-stage failure analyses
4. `08-Troubleshooting.md` — Diagnostic decision trees and debugging runbooks
5. `09-Interview-QA.md` — 10 Senior SRE/DevOps technical interview scenarios
6. `10-Hands-On-Practice.md` — Hands-on implementation labs with production blueprints
7. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible explanations
8. `12-Quick-Revision.md` — High-density cheat sheets and command reference tables

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Programming & Scripting](../05%20-%20Programming%20&%20Scripting/README.md) | [Master Index](../00-Master-Index.md) | [07 - Containers & Docker](../07%20-%20Containers%20&%20Docker/README.md) |
