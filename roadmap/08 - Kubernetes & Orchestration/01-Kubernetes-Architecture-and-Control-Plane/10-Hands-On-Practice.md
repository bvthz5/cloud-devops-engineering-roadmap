# 10 - Hands-On Practice Labs

## Lab 01: Inspecting Static Pod Manifests on a Control Plane

### Objective
Understand how Kubernetes bootstraps its own control plane components via static pods managed directly by `kubelet`.

### Steps:
```bash
# 1. SSH into your control plane node
# 2. Navigate to the static pod manifest directory
cd /etc/kubernetes/manifests

# 3. View the manifest files
ls -la
# You will see:
# kube-apiserver.yaml
# kube-controller-manager.yaml
# kube-scheduler.yaml
# etcd.yaml

# 4. Inspect how kubelet runs them as static pods
crictl pods --name kube-apiserver
```

---

## Lab 02: Interacting Directly with etcd via `etcdctl`

### Steps:
```bash
# 1. Install etcdctl or use a temporary container
sudo apt-get install -y etcd-client

# 2. Query etcd for all registered pod keys
sudo ETCDCTL_API=3 etcdctl   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   get /registry/pods --prefix --keys-only | head -n 10
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
