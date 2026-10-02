# 06 - The Dockershim Deprecation Story

## 1. Why Kubernetes Removed Dockershim

In the early days of Kubernetes, Docker was the only container engine. To support it, Kubernetes maintainers wrote a temporary translation layer called **Dockershim** inside the Kubelet.

```text
Historical Kubelet Architecture (Bloated & Inefficient):
[ Kubelet ] ──► [ Dockershim ] ──► [ dockerd ] ──► [ containerd ] ──► [ runc ] ──► [ Pod ]
                                           ▲
                          (Redundant memory and socket hops!)

Modern Kubernetes Architecture (Lean & Direct):
[ Kubelet ] ──► (CRI gRPC API) ──► [ containerd / CRI-O ] ──► [ runc / crun ] ──► [ Pod ]
```

In Kubernetes v1.24, Dockershim was permanently removed, saving control plane memory and eliminating redundant socket hops. **Note: Docker images remain 100% compatible with Kubernetes!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Sandboxed & MicroVM Runtimes](./05-Sandboxed-and-MicroVM-Runtimes-gVisor-and-Kata.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
