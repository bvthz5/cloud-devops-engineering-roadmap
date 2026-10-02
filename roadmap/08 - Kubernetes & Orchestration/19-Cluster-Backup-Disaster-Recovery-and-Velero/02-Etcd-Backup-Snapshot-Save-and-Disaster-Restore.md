# 02 - Etcd Backup: Snapshot Save and Disaster Restore

## 1. Taking a Live Snapshot

```bash
# 1. Save snapshot to disk
sudo ETCDCTL_API=3 etcdctl   --endpoints=https://127.0.0.1:2379   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   snapshot save /var/backups/etcd-backup.db

# 2. Verify snapshot integrity
sudo ETCDCTL_API=3 etcdctl snapshot status /var/backups/etcd-backup.db --write-out=table
```

---

## 2. Disaster Restoration Procedure

```bash
# 1. Stop kube-apiserver by moving static pod manifest
sudo mv /etc/kubernetes/manifests/kube-apiserver.yaml /tmp/

# 2. Restore snapshot to a NEW data directory
sudo ETCDCTL_API=3 etcdctl   snapshot restore /var/backups/etcd-backup.db   --data-dir=/var/lib/etcd-restored

# 3. Update /etc/kubernetes/manifests/etcd.yaml hostPath mount to /var/lib/etcd-restored
# 4. Move kube-apiserver.yaml back to /etc/kubernetes/manifests/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - DR Strategy](./01-Kubernetes-Disaster-Recovery-Strategy-RTO-and-RPO.md) | [README](./README.md) | [03 - Velero Architecture](./03-Velero-Architecture-and-CSI-VolumeSnapshot-Integration.md) |
