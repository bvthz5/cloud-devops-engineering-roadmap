# 07 - Containers & Docker

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

Containers represent the foundational compute abstraction of modern cloud, microservices, and Platform Engineering. A Senior DevOps/Cloud Engineer or SRE must master containerization far beyond simple `docker run` commands: from Linux kernel primitives (cgroups v1/v2, namespaces, seccomp, AppArmor), storage drivers (OverlayFS), and container networking (veth pairs, iptables NAT, bridge, macvlan), to multi-stage Dockerfiles, Docker Compose orchestrations, OCI runtime standards (`containerd`, `CRI-O`, `runc`, `gVisor`, `Kata`), daemonless rootless workflows (`Podman`, `Buildah`, `Skopeo`), and deep kernel observability (cgroups v2 PSI, exit code forensics, eBPF tracing).

---

## 📌 Complete Curriculum Modules

| # | Module | Core Focus & Engineering Scope | Status |
|---|---|---|:---:|
| 01 | [**01. Container Fundamentals (cgroups & Namespaces)**](./01-Container-Fundamentals-Cgroups-Namespaces/README.md) | Linux namespaces (`pid, net, mnt, ipc, uts, user, cgroup`), cgroups v1 vs v2 unified hierarchy, chroot/pivot_root, and rootless user namespace ID remapping. | ✅ Complete |
| 02 | [**02. Docker Architecture & CLI**](./02-Docker-Architecture-and-CLI/README.md) | Client-Server daemon architecture, Docker Engine REST API, `/var/run/docker.sock` security risks, containerd, runc, and CLI mastery. | ✅ Complete |
| 03 | [**03. Dockerfile Best Practices & Multi-Stage**](./03-Dockerfile-Best-Practices-and-Multi-Stage/README.md) | Multi-stage compilation builds, BuildKit cache mounts (`--mount=type=cache`), layer ordering optimization, `.dockerignore`, and Distroless/Scratch bases. | ✅ Complete |
| 04 | [**04. Docker Storage & Volumes**](./04-Docker-Storage-and-Volumes/README.md) | OverlayFS kernel mechanics (`lowerdir, upperdir, merged, workdir`), Named Volumes vs Bind Mounts vs `tmpfs`, copy-on-write overhead, and stateful backup strategies. | ✅ Complete |
| 05 | [**05. Docker Networking**](./05-Docker-Networking/README.md) | Virtual Ethernet (`veth`) pairs, Linux bridges (`docker0`), embedded DNS server, iptables MASQUERADE & port forwarding, Host, Macvlan, and Overlay driver design. | ✅ Complete |
| 06 | [**06. Docker Compose Multi-Container Apps**](./06-Docker-Compose-Multi-Container-Apps/README.md) | Multi-service orchestration, YAML specification v2/v3, `depends_on` condition checks (`service_healthy`), secrets, environment hierarchies, and profile management. | ✅ Complete |
| 07 | [**07. Container Security & Image Scanning**](./07-Container-Security-and-Image-Scanning/README.md) | Principle of least privilege, non-root USER directives, Linux capabilities (`cap-drop=ALL`), seccomp, AppArmor profiles, Trivy vulnerability scanning, and Docker CIS Benchmarks. | ✅ Complete |
| 08 | [**08. Container Registries (DockerHub, ECR, GHCR)**](./08-Container-Registries-DockerHub-ECR-GHCR/README.md) | OCI Registry Spec v1.1, Docker Hub rate limits, AWS ECR token lifecycle, GitHub Container Registry (GHCR), image tagging immutability, Cosign signing, and multi-arch manifests. | ✅ Complete |
| 09 | [**09. OCI Standards & Container Runtimes**](./09-OCI-Standards-and-Container-Runtimes/README.md) | OCI Image, Runtime, and Distribution Specs; `containerd` architecture & CRI plugin; `CRI-O` lightweight design; `runc` vs `crun`; sandboxed microVMs (`gVisor`, `Kata`); and Dockershim deprecation. | ✅ Complete |
| 10 | [**10. Podman, Buildah & Skopeo Daemonless Stack**](./10-Podman-Buildah-and-Skopeo-Daemonless-Stack/README.md) | Daemonless fork/exec execution model, rootless containers (`/etc/subuid`, `/etc/subgid`), Podman pods & Kubernetes YAML generation, systemd Quadlets, Buildah scripting, and Skopeo remote inspection. | ✅ Complete |
| 11 | [**11. Container Observability & Troubleshooting**](./11-Container-Observability-and-Troubleshooting/README.md) | cgroups v2 Pressure Stall Information (PSI), container exit code forensics (137 OOMKilled, 139 Segfault, 126, 127), non-blocking logging drivers, cAdvisor + Prometheus, ephemeral debugging, and eBPF tracing. | ✅ Complete |

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
| [06 - Web Servers & Reverse Proxies](../06%20-%20Web%20Servers%20&%20Reverse%20Proxies/README.md) | [Master Index](../00-Master-Index.md) | [08 - Kubernetes & Orchestration](../08%20-%20Kubernetes%20&%20Orchestration/README.md) |
