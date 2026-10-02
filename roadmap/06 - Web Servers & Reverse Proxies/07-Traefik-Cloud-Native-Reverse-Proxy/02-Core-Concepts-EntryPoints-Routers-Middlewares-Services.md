# 02 - Core Concepts: EntryPoints, Routers, Middlewares, and Services

## 1. The 4 Fundamental Pillars

```text
[ Client Request ]
       │
       ▼
 [ EntryPoint ]   (e.g., :443 websecure - The network port listening for connections)
       │
       ▼
   [ Router ]     (Evaluates Rule: Host(`api.example.com`) && PathPrefix(`/v1`))
       │
       ▼
 [ Middlewares ]  (Modifies request: StripPrefix, RateLimit, BasicAuth, Headers)
       │
       ▼
   [ Service ]    (Load balances across target backend container IPs and ports)
       │
       ▼
[ Backend Containers ]
```

---

## 2. Powerful Router Rule Primitives

Traefik evaluates incoming HTTP attributes using composable Boolean expressions:

```yaml
# Examples of Router Rules:
rule: "Host(`example.com`) || Host(`www.example.com`)"
rule: "Host(`api.example.com`) && PathPrefix(`/users`)"
rule: "Host(`service.internal`) && Headers(`X-Beta-Tester`, `true`)"
rule: "Host(`app.example.com`) && ClientIP(`10.0.0.0/8`)"
```

---

## 3. Middleware Pipelines

Middlewares transform the request before it reaches the backend, or modify the response before returning to the client:
- **`stripPrefix`**: Strips `/api` prefix before passing to backend.
- **`rateLimit`**: Enforces token bucket rate limiting.
- **`basicAuth`**: Enforces HTTP Basic Authentication.
- **`compress`**: Gzip/Brotli response compression.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Traefik Architecture and Dynamic Discovery](./01-Traefik-Architecture-and-Dynamic-Discovery.md) | [Index](../../../README.md) | [03 - Docker and Docker Compose Integration →](./03-Docker-and-Docker-Compose-Integration.md) |
