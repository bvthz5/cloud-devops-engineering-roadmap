# 05 - Lightweight Edge Kubernetes with K3s and MicroK8s

## 1. K3s: 512MB RAM Production Engine

**K3s** (created by Rancher) packages the entire Kubernetes control plane and worker components into a single `<100MB` binary:
- Replaces `etcd` with embedded SQLite (or external PostgreSQL/MySQL) for single-master, or embedded Raft for multi-master.
- Replaces in-tree cloud providers and storage drivers with lightweight CSI/CNI.
- Ships with Traefik Ingress, Flannel CNI, and CoreDNS pre-installed.

```bash
# 1. Install K3s single-node in 30 seconds
curl -sfL https://get.k3s.io | sh -

# 2. Access cluster
sudo k3s kubectl get nodes

# 3. Retrieve node join token for worker nodes
sudo cat /var/lib/rancher/k3s/server/node-token
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Local Cluster Setup with Kind and Minikube](./04-Local-Cluster-Setup-with-Kind-and-Minikube.md) | [Index](../../../README.md) | [06 - Production Bootstrap with Kubeadm →](./06-Production-Bootstrap-with-Kubeadm.md) |
