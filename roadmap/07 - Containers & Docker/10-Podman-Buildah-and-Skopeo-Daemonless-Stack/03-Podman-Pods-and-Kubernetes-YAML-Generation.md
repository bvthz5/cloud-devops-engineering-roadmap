# 03 - Podman Pods and Kubernetes YAML Generation

## 1. Pods on a Single Host

Podman natively supports the Kubernetes **Pod** concept without requiring a Kubernetes cluster. Containers within a Pod share the same network namespace (localhost), IPC namespace, and cgroup:

```bash
# 1. Create a pod with published ports
podman pod create --name web-pod -p 8080:80

# 2. Add containers to the pod (they communicate via localhost!)
podman run -d --pod web-pod --name web nginx:alpine
podman run -d --pod web-pod --name app-worker my-worker-image

# 3. Export as pure Kubernetes Pod YAML!
podman generate kube web-pod > pod.yaml

# 4. Play/Deploy Kubernetes YAML locally!
podman play kube pod.yaml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Rootless Podman and User Namespaces](./02-Rootless-Podman-and-User-Namespaces.md) | [Index](../../../README.md) | [04 - Systemd Integration and Podman Quadlets →](./04-Systemd-Integration-and-Podman-Quadlets.md) |
