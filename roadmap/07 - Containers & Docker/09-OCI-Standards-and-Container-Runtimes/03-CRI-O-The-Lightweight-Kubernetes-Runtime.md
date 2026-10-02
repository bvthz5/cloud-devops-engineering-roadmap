# 03 - CRI-O: The Lightweight Kubernetes Runtime

## 1. Purpose-Built for Kubernetes

While `containerd` supports standalone CLI tools, Swarm, and general-purpose development, **CRI-O** was engineered exclusively to serve as the runtime for Kubernetes:
- Implements the Kubernetes Container Runtime Interface (CRI) with zero unnecessary features.
- Tightly scoped and tied to Kubernetes release versions (CRI-O 1.29 matches Kubernetes 1.29).
- The default container runtime in Red Hat OpenShift.

```bash
# Diagnostic inspection tool for any CRI-compliant runtime (containerd or CRI-O)
sudo crictl pods
sudo crictl ps
sudo crictl logs <container-id>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Containerd Architecture and CRI Plugin](./02-Containerd-Architecture-and-CRI-Plugin.md) | [Index](../../../README.md) | [04 - Low Level Runtimes runc vs crun vs youki →](./04-Low-Level-Runtimes-runc-vs-crun-vs-youki.md) |
