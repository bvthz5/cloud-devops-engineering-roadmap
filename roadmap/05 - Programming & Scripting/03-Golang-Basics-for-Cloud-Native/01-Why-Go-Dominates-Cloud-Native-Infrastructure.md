# 01 - Why Go Dominates Cloud-Native Infrastructure

## 1. The Cloud-Native Ecosystem is Built on Go

Look at the modern cloud architecture stack:
- **Container Runtimes:** Docker, containerd, CRI-O, Podman.
- **Orchestration:** Kubernetes, Nomad.
- **Infrastructure as Code:** Terraform, Pulumi.
- **Observability:** Prometheus, Grafana, Jaeger.
- **Networking:** CNI plugins, CoreDNS, MetalLB.

---

## 2. The 4 Engineering Advantages of Go

1. **Static Single Binary:** Go compiles code, runtime, and all libraries into a **single, self-contained binary**. You can copy it into a bare `scratch` Docker container (5-15 MB total image size) with zero installed system packages.
2. **Instant Startup Time:** Unlike Java or Python runtimes that take seconds to load VMs and parse scripts, Go binaries start in **under 10 milliseconds**—ideal for serverless and Kubernetes pod autoscaling.
3. **Low Memory Footprint:** A Go HTTP service idles at 15 MB RAM, compared to 150-300 MB for equivalent Python/Java servers.
4. **First-Class Concurrency:** Goroutines cost only ~2 KB of memory compared to ~1 MB for an OS thread.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Go Syntax](./02-Go-Syntax-Structs-Pointers-and-Interfaces.md) |
