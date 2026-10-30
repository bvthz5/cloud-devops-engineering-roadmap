# 02 - Rootless Podman and User Namespaces

## 1. User Namespaces and `/etc/subuid`

Rootless Podman allows non-root users to run containers securely using **User Namespaces**:
- The user is UID 0 (root) *inside* the container, but maps to an unprivileged subordinate UID (e.g. 100000-165535) *outside* on the host.

```text
Host /etc/subuid:
developer:100000:65536

UID Inside Container:        UID on Physical Host:
UID 0 (root inside container)  ──► UID 1000 (developer on host)
UID 1                          ──► UID 100000 on host
UID 2                          ──► UID 100001 on host
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - The Daemonless Container Architecture](./01-The-Daemonless-Container-Architecture.md) | [Index](../../../README.md) | [03 - Podman Pods and Kubernetes YAML Generation →](./03-Podman-Pods-and-Kubernetes-YAML-Generation.md) |
