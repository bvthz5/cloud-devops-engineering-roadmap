# 02 - containerd Architecture and CRI Plugin

## 1. The Industry Standard Container Supervisor

**containerd** is a CNCF graduated container runtime that manages the complete container lifecycle of its host system: image transfer and storage, container execution and supervision, low-level storage attachments, and network attachments.

```text
[ Kubernetes Kubelet ]
          │ (gRPC: /run/containerd/containerd.sock)
          ▼
[ containerd ] ──► [ CRI Plugin ] (Translates Kubelet CRI requests)
          │
          ├──► Image Service (Pulls & unpacks OCI layers)
          └──► Runtime Service (Supervises containers via runc / crun)
```

---

## 2. Tooling: `ctr` vs `nerdctl`

- **`ctr`**: Low-level developer CLI bundled with containerd. Not intended for end-users (unfriendly UX, requires explicit namespaces like `-n k8s.io`).
- **`nerdctl`**: Docker-compatible CLI for containerd that supports `docker-compose`, multi-platform builds, and rootless containers.

```bash
# List containers in the Kubernetes namespace using crictl/ctr
sudo ctr -n k8s.io containers list

# Docker-compatible CLI targeting containerd directly
nerdctl run -d -p 8080:80 nginx:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Open Container Initiative OCI Specifications](./01-Open-Container-Initiative-OCI-Specifications.md) | [Index](../../../README.md) | [03 - CRI O The Lightweight Kubernetes Runtime →](./03-CRI-O-The-Lightweight-Kubernetes-Runtime.md) |
