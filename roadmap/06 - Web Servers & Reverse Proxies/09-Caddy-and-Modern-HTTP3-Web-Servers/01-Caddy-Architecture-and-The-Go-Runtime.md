# 01 - Caddy Architecture and The Go Runtime

## 1. Memory Safety and Go Concurrency

Unlike traditional web servers written in C (Nginx, Apache, HAProxy), Caddy is written entirely in **Go**. This architectural choice provides:
1. **Memory Safety**: Immune to memory corruption bugs, buffer overflows, and use-after-free vulnerabilities that plague C-based web servers.
2. **Goroutine Concurrency**: High-concurrency network handling powered by the Go runtime's M:N green thread scheduler.
3. **Pluggable Architecture**: Easily extended with bespoke modules (DNS providers, rate limiters, auth middleware) compiled via `xcaddy`.

---

## 2. Dual Configuration Planes: Caddyfile vs JSON API

Caddy is natively configured via a structured **JSON Document**. The popular **Caddyfile** is simply an ergonomic adapter that Caddy parses and compiles into JSON internally:

```text
[ Developer writes Caddyfile ] ──► [ Caddyfile Adapter ] ──► [ Internal JSON Config Engine ]
                                                                        ▲
[ Automation / API via HTTP ] ──► [ Admin REST API (:2019) ] ───────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Automatic HTTPS & Internal PKI](./02-Automatic-HTTPS-and-Internal-PKI.md) |
