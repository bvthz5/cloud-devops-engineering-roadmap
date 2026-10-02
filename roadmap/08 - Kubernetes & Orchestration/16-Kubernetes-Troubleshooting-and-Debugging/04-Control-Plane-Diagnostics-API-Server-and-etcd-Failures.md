# 04 - Control Plane Diagnostics: API Server and etcd Failures

## 1. Master Node Triage Runbook

When `kubectl` commands fail with `connection refused`:

```bash
# 1. Check if control plane static pods are running via container runtime directly
sudo crictl ps -a | grep -E "kube-apiserver|etcd"

# 2. Check API server container logs
sudo crictl logs $(sudo crictl ps -a --name kube-apiserver -q)

# 3. Check etcd health directly
sudo ETCDCTL_API=3 etcdctl   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   endpoint health
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Network Debugging DNS Resolution Failures and Packet Loss](./03-Network-Debugging-DNS-Resolution-Failures-and-Packet-Loss.md) | [Index](../../../README.md) | [05 - Ephemeral Debug Containers and Kubectl Debug →](./05-Ephemeral-Debug-Containers-and-Kubectl-Debug.md) |
