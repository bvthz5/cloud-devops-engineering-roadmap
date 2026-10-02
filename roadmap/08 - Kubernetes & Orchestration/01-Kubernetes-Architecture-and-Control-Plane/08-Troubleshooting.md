# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Control Plane Failure

```text
[ Symptom: kubectl returns "The connection to the server <ip>:6443 was refused" ]
                         │
                         ▼
        Can you SSH into the Control Plane node?
        ├── NO  ──► Check cloud VM state, hypervisor, or network security groups.
        └── YES ──► Check if Docker / containerd runtime is active:
                    systemctl status containerd
                         │
                         ▼
        Check if kubelet is running:
        systemctl status kubelet
        ├── FAILED ──► Inspect logs: journalctl -u kubelet -e -n 100
        │              Look for: certificate expiration, invalid flags, cgroup mismatch
        └── ACTIVE ──► Inspect Static Pod containers:
                       crictl ps -a | grep -E "kube-apiserver|etcd"
                         │
                         ▼
        Is kube-apiserver crashing?
        crictl logs <apiserver-container-id>
        ├── "failed to connect to etcd" ──► Inspect etcd container logs
        └── "certificate signed by unknown authority" ──► Regenerate certs with kubeadm
```

---

## Critical Commands for Control Plane Inspection

```bash
# 1. Inspect kubelet status and recent failures
sudo journalctl -u kubelet -n 50 --no-pager

# 2. Check running static control plane containers via crictl
sudo crictl ps

# 3. Check API server logs directly from container
sudo crictl logs $(sudo crictl ps --name kube-apiserver -q)

# 4. Check certificate expiration dates
sudo kubeadm certs check-expiration
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
