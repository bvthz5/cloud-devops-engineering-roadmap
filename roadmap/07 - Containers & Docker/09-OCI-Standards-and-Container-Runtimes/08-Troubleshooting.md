# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Kubernetes Containers with `crictl`

```bash
# Configure crictl endpoint
cat <<EOF | sudo tee /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
EOF

# List all pods and containers
sudo crictl pods
sudo crictl ps

# Inspect container health and cgroup status
sudo crictl inspect <container-id>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
