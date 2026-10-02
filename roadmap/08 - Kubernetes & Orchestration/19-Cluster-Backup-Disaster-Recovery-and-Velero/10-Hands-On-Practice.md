# 10 - Hands-On Practice Labs

## Lab: Taking and Validating an etcd Snapshot

```bash
# 1. Export snapshot
sudo ETCDCTL_API=3 etcdctl   --endpoints=https://127.0.0.1:2379   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   snapshot save /tmp/test-etcd.db

# 2. Verify snapshot status
sudo ETCDCTL_API=3 etcdctl snapshot status /tmp/test-etcd.db -w table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
