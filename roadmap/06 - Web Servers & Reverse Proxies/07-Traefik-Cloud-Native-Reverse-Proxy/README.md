# 07 - Traefik Cloud-Native Reverse Proxy

Traefik (pronounced "traffic") is a modern HTTP reverse proxy and ingress controller designed specifically for microservices, Docker containers, and Kubernetes. Unlike traditional web servers that require manually editing static configuration files and reloading processes, Traefik integrates directly with container orchestrators to **dynamically discover services and configure routing in real time**.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Traefik Architecture & Dynamic Discovery](./01-Traefik-Architecture-and-Dynamic-Discovery.md) | Cloud-native design in Go, static vs dynamic config, automated provider polling. |
| 02 | [Core Concepts: EntryPoints, Routers, Middlewares & Services](./02-Core-Concepts-EntryPoints-Routers-Middlewares-Services.md) | The 4 core building blocks, routing rules (`Host`, `PathPrefix`), middleware pipelines. |
| 03 | [Docker and Docker Compose Integration](./03-Docker-and-Docker-Compose-Integration.md) | Label-based discovery (`traefik.http.routers.*`), container port inference, Swarm mode. |
| 04 | [Kubernetes Ingress & Gateway API with Traefik](./04-Kubernetes-Ingress-and-Gateway-API-with-Traefik.md) | Standard Kube Ingress vs Traefik `IngressRoute` CRD, canary traffic splitting. |
| 05 | [Automated Let's Encrypt TLS Management](./05-Automated-Lets-Encrypt-TLS-Management.md) | Native ACME engine, certificatesResolvers, HTTP-01 vs DNS-01, `acme.json` security. |
| 06 | [Observability: Tracing, Metrics & Dashboard](./06-Observability-Tracing-and-Dashboard.md) | Prometheus metrics, OpenTelemetry tracing, Jaeger, Traefik dashboard security. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: `acme.json` permission error, exposed Docker socket vulnerability. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Enabling `DEBUG` log level, inspecting dynamic provider registry, routing collisions. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior Platform Engineer / DevOps questions on Traefik vs Nginx and IngressRoute CRDs. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Docker Compose auto-discovery; Lab 2: Kubernetes IngressRoute canary; Lab 3: ACME DNS-01. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for traefik.yml syntax, Docker labels, and IngressRoute CRDs. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - HAProxy Load Balancing](../06-HAProxy-High-Performance-Load-Balancing/README.md) | [README](./README.md) | [01 - Traefik Architecture](./01-Traefik-Architecture-and-Dynamic-Discovery.md) |
