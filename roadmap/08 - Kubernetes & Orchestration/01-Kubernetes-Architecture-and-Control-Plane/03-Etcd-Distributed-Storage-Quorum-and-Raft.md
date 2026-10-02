# 03 - Etcd Distributed Storage, Quorum, and Raft

## 1. Raft Consensus in Kubernetes

`etcd` is an open-source, distributed, consistent key-value store. It implements the **Raft consensus algorithm** to ensure that all nodes in an etcd cluster agree on the exact sequence of state modifications.

```text
        +---------------+
        |  API Server   |
        +-------+-------+
                | Writes KV
                v
        +---------------+
        |  etcd Leader  | (Accepts all writes)
        +---+-------+---+
            |       |
    Append  |       | Append
    Entries |       | Entries
            v       v
+-------------+   +-------------+
| etcd Member |   | etcd Member | (Followers replicate log)
| (Follower)  |   | (Follower)  |
+-------------+   +-------------+
```

---

## 2. Quorum Calculation & Cluster Sizing

To tolerate node failures while preventing **Split-Brain**, etcd requires a strict majority vote ($Q$):

$$	ext{Quorum} = \left\lfloor rac{N}{2} ightfloor + 1$$

| Cluster Size ($N$) | Quorum ($Q$) | Max Failures Tolerated ($F = rac{N-1}{2}$) |
|:---:|:---:|:---:|
| **1** | 1 | 0 (No HA, dev only) |
| **3** | 2 | **1** |
| **5** | 3 | **2** |
| **7** | 4 | **3** |

> [!WARNING]
> **Never use an even number of etcd members!** A 4-node cluster requires 3 nodes for quorum—the exact same tolerance as a 3-node cluster, but with an increased risk of split-brain partition failure.

---

## 3. Production etcd Operations (`etcdctl`)

```bash
# 1. Check cluster health and leadership
ETCDCTL_API=3 etcdctl   --endpoints=https://127.0.0.1:2379   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   endpoint status --write-out=table

# 2. Perform live snapshot backup
ETCDCTL_API=3 etcdctl   --endpoints=https://127.0.0.1:2379   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   snapshot save /var/backups/etcd-snapshot-$(date +%Y%m%d).db

# 3. Defragment etcd database to reclaim free space
ETCDCTL_API=3 etcdctl   --endpoints=https://127.0.0.1:2379   --cacert=/etc/kubernetes/pki/etcd/ca.crt   --cert=/etc/kubernetes/pki/etcd/server.crt   --key=/etc/kubernetes/pki/etcd/server.key   defrag
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Node Components](./02-Node-Components-Kubelet-KubeProxy-and-CRI.md) | [README](./README.md) | [04 - API Request Flow](./04-Kubernetes-API-Request-Flow-Authentication-Admission.md) |
