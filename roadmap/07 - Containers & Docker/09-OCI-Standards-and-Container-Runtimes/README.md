# 09 - OCI Standards and Container Runtimes

The container ecosystem has matured far beyond Docker into an open, standardized stack governed by the **Open Container Initiative (OCI)** and Cloud Native Computing Foundation (CNCF). In production Kubernetes clusters, the Kubelet communicates with specialized container runtimes like **containerd** and **CRI-O**, which in turn invoke low-level runtimes (**runc**, **crun**) or sandboxed microVMs (**gVisor**, **Kata Containers**).

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [OCI Specifications: Image, Runtime & Distribution](./01-Open-Container-Initiative-OCI-Specifications.md) | OCI standards, decoupling containers from Docker, Runtime Spec (`config.json`), Image Spec. |
| 02 | [containerd Architecture & CRI Plugin](./02-Containerd-Architecture-and-CRI-Plugin.md) | High-performance container supervisor, CRI plugin, `ctr` vs `nerdctl` CLI usage. |
| 03 | [CRI-O: The Lightweight Kubernetes Runtime](./03-CRI-O-The-Lightweight-Kubernetes-Runtime.md) | Purpose-built runtime for Kubernetes, minimal footprint, `crictl` diagnostic tool. |
| 04 | [Low-Level Runtimes: runc vs crun vs youki](./04-Low-Level-Runtimes-runc-vs-crun-vs-youki.md) | Reference `runc` (Go) vs C-based `crun` (low memory, 50x faster start) vs `youki` (Rust). |
| 05 | [Sandboxed & MicroVM Runtimes: gVisor & Kata](./05-Sandboxed-and-MicroVM-Runtimes-gVisor-and-Kata.md) | Kernel interception with gVisor (`runsc`), hardware virtualization with Kata Containers. |
| 06 | [The Dockershim Deprecation Story](./06-The-Dockershim-Deprecation-Story.md) | Why Kubernetes removed Dockershim in v1.24, architectural simplification, migration guide. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Dockershim removal upgrade crash, gVisor syscall incompatibility. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging with `crictl`, inspecting CRI socket timeouts, containerd event streams. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior Platform Engineer / SRE interview questions on OCI runtimes, CRI, and sandboxes. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Managing containers with nerdctl; Lab 2: Inspecting pods with crictl; Lab 3: runc spec. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for containerd config, crictl commands, and OCI specifications. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Container Registries](../08-Container-Registries-DockerHub-ECR-GHCR/README.md) | [README](./README.md) | [01 - OCI Specifications](./01-Open-Container-Initiative-OCI-Specifications.md) |
