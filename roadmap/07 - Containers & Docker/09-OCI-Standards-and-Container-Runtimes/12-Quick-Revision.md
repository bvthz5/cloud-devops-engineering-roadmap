# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# crictl CLI commands (Kubernetes runtime debugging)
crictl pods
crictl ps
crictl images
crictl logs <id>

# nerdctl (Docker CLI alternative for containerd)
nerdctl run -d -p 80:80 nginx:alpine
```

```text
OCI Ecosystem Stack:
Kubelet -> (CRI) -> containerd / CRI-O -> (OCI Runtime Spec) -> runc / crun -> Linux Kernel
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [10 - Podman, Buildah & Skopeo](../10-Podman-Buildah-and-Skopeo-Daemonless-Stack/README.md) |
