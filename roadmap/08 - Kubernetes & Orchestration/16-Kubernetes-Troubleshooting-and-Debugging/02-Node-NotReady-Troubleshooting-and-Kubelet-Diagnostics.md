# 02 - Node NotReady Troubleshooting and Kubelet Diagnostics

## 1. Anatomy of a Node Transition to `NotReady`

Every 10 seconds, `kubelet` sends a node status lease heartbeat to `kube-apiserver`.
If no heartbeat is received for `node-monitor-grace-period` (default: **40 seconds**), the controller manager marks the node as `NotReady`:

```bash
# 1. SSH into the troubled node
# 2. Check kubelet systemd service
sudo systemctl status kubelet

# 3. View kubelet error logs
sudo journalctl -u kubelet -e --no-pager -n 100

# 4. Check container runtime responsiveness
sudo crictl info
sudo crictl ps
```

### Common Root Causes
- **PLEG is not healthy:** Container runtime (containerd) is deadlocked on disk I/O.
- **DiskPressure:** Root partition or container storage filesystem (`/var/lib/containerd`) > 85% full.
- **Certificate Expiration:** Kubelet client certificate in `/var/lib/kubelet/pki` has expired.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Pod Failure States CrashLoopBackOff OOMKilled ImagePullBackOff](./01-Pod-Failure-States-CrashLoopBackOff-OOMKilled-ImagePullBackOff.md) | [Index](../../../README.md) | [03 - Network Debugging DNS Resolution Failures and Packet Loss →](./03-Network-Debugging-DNS-Resolution-Failures-and-Packet-Loss.md) |
