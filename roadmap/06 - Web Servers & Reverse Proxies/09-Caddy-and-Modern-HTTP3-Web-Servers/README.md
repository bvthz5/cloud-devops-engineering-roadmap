# 09 - Caddy and Modern HTTP/3 Web Servers

Caddy is an enterprise-grade, memory-safe web server and reverse proxy written in Go. Famous for pioneering **Automatic HTTPS by default**, Caddy simplifies web infrastructure by automating TLS certificate acquisition and renewal, natively supporting **HTTP/3 (QUIC)**, providing a human-friendly configuration format (Caddyfile), and exposing a dynamic JSON REST administration API.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Caddy Architecture & The Go Runtime](./01-Caddy-Architecture-and-The-Go-Runtime.md) | Go memory safety, modular architecture via `xcaddy`, Caddyfile vs JSON API. |
| 02 | [Automatic HTTPS & Internal PKI](./02-Automatic-HTTPS-and-Internal-PKI.md) | Zero-config TLS via ACME (Let's Encrypt/ZeroSSL), local CA for dev, On-Demand TLS for SaaS. |
| 03 | [Native HTTP/3, QUIC & 0-RTT](./03-Native-HTTP3-QUIC-and-0-RTT.md) | HTTP/3 over UDP/QUIC, eliminating head-of-line blocking, connection migration, 0-RTT. |
| 04 | [Caddyfile Syntax, Directives & Snippets](./04-Caddyfile-Syntax-Directives-and-Snippets.md) | `reverse_proxy`, matchers (`@custom`), reusable snippets, environment variable interpolation. |
| 05 | [Dynamic JSON API & Zero-Downtime Config](./05-Dynamic-JSON-API-and-Zero-Downtime-Config.md) | Admin API `:2019`, dynamic configuration patching with HTTP POST, JSON pointer navigation. |
| 06 | [Caddy as a Kubernetes Ingress & Edge Proxy](./06-Caddy-as-a-Kubernetes-Ingress-and-Container-Edge.md) | Caddy Ingress Controller, Docker container edge, compiling custom modules with `xcaddy`. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: On-Demand TLS rate-limit exhaustion, UDP buffer drops under HTTP/3. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | `caddy validate`, `caddy reload`, inspecting storage paths, debugging certificate renewal failures. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps questions on Caddy vs Nginx, HTTP/3 QUIC, and On-Demand TLS. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Reverse proxy with automatic HTTPS; Lab 2: Dynamic config via JSON API; Lab 3: HTTP/3. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for Caddyfile syntax, matchers, and admin API calls. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Envoy Proxy](../08-Envoy-Proxy-and-Service-Mesh-Data-Plane/README.md) | [README](./README.md) | [01 - Caddy Architecture](./01-Caddy-Architecture-and-The-Go-Runtime.md) |
