# 03 - Etcd Distributed Storage, Quorum, and Raft

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to `etcd`, exploring Raft consensus, leader election, log replication, quorum mathematics, bbolt B+tree storage, compaction, defragmentation, and disaster recovery.

---

## 🏛️ 1. What Is `etcd` & Why Is It the Foundation of Kubernetes?

In any distributed system, maintaining a single, consistent version of reality across multiple independent servers is notoriously difficult. If two control plane nodes simultaneously attempt to schedule workloads during a network partition, the cluster risks **Split-Brain**—a catastrophic state where competing nodes corrupt the cluster with conflicting decisions.

**`etcd`** is the distributed, strongly consistent key-value store that acts as the single source of truth for the entire Kubernetes cluster. Every Pod definition, Service endpoint, Secret, ConfigMap, and resource status is stored within `etcd`.

`etcd` was originally created in 2013 by the CoreOS engineering team (led by Alex Polvi and Brandon Philips). Its name is derived from the Unix `/etc` directory (where system configurations are stored) combined with `d` for "distributed". In 2018, CoreOS donated `etcd` to the Cloud Native Computing Foundation (CNCF), where it graduated as a top-tier project.

---

## ⚙️ 2. How the Raft Consensus Algorithm Works Internally

To guarantee that all nodes agree on the exact sequence of cluster state modifications without centralized database bottlenecks, `etcd` implements the **Raft Consensus Algorithm** (designed by Diego Ongaro and John Ousterhout at Stanford University).

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   RAFT REPLICATION & CONSENSUS WORKFLOW                │
└────────────────────────────────────────────────────────────────────────┘

 1. Client Write Request:
    kube-apiserver ──► POST /api/v1/pods
         │
         ▼ (Writes exclusively to the elected Leader)
 ┌──────────────────────────────────────────────────────────────────────┐
 │ etcd Node 1: LEADER (Term 4)                                         │
 │ - Appends mutation to Write-Ahead Log (WAL) on disk                  │
 └──────────────────┬───────────────────────────────┬───────────────────┘
                    │                               │
                    │ AppendEntries RPC             │ AppendEntries RPC
                    ▼                               ▼
 ┌──────────────────────────────────────┐ ┌─────────────────────────────┐
 │ etcd Node 2: FOLLOWER                │ │ etcd Node 3: FOLLOWER       │
 │ - Appends entry to local WAL         │ │ - Appends entry to local WAL│
 │ - Acknowledges success to Leader     │ │ - Network packet delayed!   │
 └──────────────────┬───────────────────┘ └─────────────────────────────┘
                    │
                    ▼ (Quorum Achieved! 2 out of 3 nodes acknowledged!)
 ┌──────────────────────────────────────────────────────────────────────┐
 │ etcd Node 1: LEADER Commits Transaction                              │
 │ - Applies transaction to persistent bbolt B+tree database            │
 │ - Sends HTTP 201 Created response back to kube-apiserver             │
 └──────────────────────────────────────────────────────────────────────┘
```

### 2.1 The Three States of an etcd Node
At any given moment, every member in an `etcd` cluster exists in one of three states:
1. **Leader:** The single node responsible for receiving all client write requests, appending log entries, and orchestrating replication across followers.
2. **Follower:** Passive members that receive heartbeats and log entries from the leader. If followers do not hear from a leader within a randomized election timeout, they assume the leader has died.
3. **Candidate:** A follower whose election timer has expired. It increments the cluster **Term**, votes for itself, and broadcasts `RequestVote` RPCs to peer nodes. If it secures a majority vote, it becomes the new Leader.

### 2.2 The Two-Phase Log Commit Lifecycle
1. **Log Replication:** When the leader receives a write from the API server, it writes the mutation to its local disk-backed **Write-Ahead Log (WAL)**. It then broadcasts `AppendEntries` RPCs to all followers.
2. **Commit Phase:** Once a strict **Quorum** (majority) of followers acknowledge that the entry has been safely written to their local WALs, the leader commits the transaction to its persistent database and notifies followers that the entry is permanent.

---

## 📐 3. Quorum Mathematics & Why Odd Cluster Sizes Are Mandatory

To prevent split-brain during network partitions, `etcd` enforces the strict mathematical law of **Majority Quorum ($Q$)**:

$$\text{Quorum} = \left\lfloor \frac{N}{2} \right\rfloor + 1$$

Where $N$ represents the total number of members in the cluster.

| Cluster Size ($N$) | Quorum ($Q$) Needed for Writes | Maximum Tolerated Node Failures ($F$) | Architectural Recommendation |
| :---: | :---: | :---: | :--- |
| **1** | 1 | **0** | Development / Test only (Zero high availability). |
| **3** | 2 | **1** | **Standard Production HA:** Survives 1 node loss. |
| **5** | 3 | **2** | **Mission-Critical Enterprise:** Survives 2 node losses. |
| **7** | 4 | **3** | Very large clusters (Increased Raft sync latency). |

### ⚠️ The Fatal Fallacy of Even-Numbered Clusters (Why 4 Nodes is Worse Than 3):
A common mistake among junior infrastructure engineers is adding a fourth node to a 3-node cluster, believing it increases redundancy:
- In a **3-node cluster**, $Q = \lfloor 3/2 \rfloor + 1 = 2$. It survives **1 failure**.
- In a **4-node cluster**, $Q = \lfloor 4/2 \rfloor + 1 = 3$. It still survives **only 1 failure**!
- Furthermore, if a network partition splits a 4-node cluster into two equal halves ($2 \text{ nodes} \leftrightarrow 2 \text{ nodes}$), **neither side has 3 nodes**. The entire cluster freezes completely! Always use odd numbers ($3, 5$).

---

## 💾 4. Internal Storage Architecture: B-Trees and bbolt

`etcd` maintains its data using a dual-tiered storage model designed for sub-millisecond retrieval:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   ETCD DUAL-TIER STORAGE ENGINE                        │
└────────────────────────────────────────────────────────────────────────┘

 ┌──────────────────────────────────────────────────────────────────────┐
 │ Memory Tier: In-Memory B-Tree Index                                  │
 │ Maps user keys (e.g. /registry/pods/default/nginx) to Revisions     │
 │ (e.g. Created at Revision 1024, Modified at Revision 1089)           │
 └──────────────────────────────────┬───────────────────────────────────┘
                                    │
                                    ▼ (Pointed to exact revision)
 ┌──────────────────────────────────────────────────────────────────────┐
 │ Disk Tier: bbolt B+Tree Database Engine (member/snap/db)             │
 │ Key: Revision Number (e.g. 1089) ──► Value: Protobuf Encoded Object  │
 │ - Multi-Version Concurrency Control (MVCC)                           │
 │ - Preserves historical change states for Watch streams               │
 └──────────────────────────────────────────────────────────────────────┘
```

### 4.1 Compaction and Defragmentation
Because `etcd` uses MVCC, modifying a key does not overwrite the existing data; it creates a new revision. Over time, millions of revisions accumulate, threatening the configured storage quota (default: 2 GB, recommended enterprise limit: 8 GB).

1. **Compaction (`etcdctl compact`):** Removes historical revisions older than a specified threshold. However, compaction only frees logical space inside the bbolt database; it does not shrink the physical file size on host disk.
2. **Defragmentation (`etcdctl defrag`):** Rewrites the bbolt database pages sequentially, eliminating internal fragmentation and releasing unused disk blocks back to the Linux host filesystem.

---

## 🛠️ 5. Production etcd Administration & Disaster Recovery CLI

```bash
# 1. Check health, response latency, and leadership across all endpoints
sudo ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  endpoint status --write-out=table

# 2. Capture a live, non-disruptive snapshot backup of the cluster
sudo ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /var/backups/etcd-backup-$(date +%Y%m%d).db

# 3. Verify integrity of the snapshot archive
sudo ETCDCTL_API=3 etcdctl snapshot status /var/backups/etcd-backup-20261010.db --write-out=table

# 4. Perform live database defragmentation to reclaim disk blocks
sudo ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  defrag
```

### 5.1 Disaster Recovery: Restoring an etcd Snapshot
If all control plane nodes fail, operators restore cluster state using `etcdctl snapshot restore`:
```bash
# Stop all control plane static pods temporarily
sudo mv /etc/kubernetes/manifests/*.yaml /tmp/

# Restore the snapshot into a fresh data directory
sudo ETCDCTL_API=3 etcdctl snapshot restore /var/backups/etcd-backup-20261010.db \
  --data-dir=/var/lib/etcd-restored

# Point etcd static pod to /var/lib/etcd-restored and restore manifests
sudo mv /tmp/*.yaml /etc/kubernetes/manifests/
```

---

## 📋 6. Summary and Navigation

`etcd` guarantees total transactional consistency and fault tolerance at the heart of Kubernetes. By leveraging the Raft consensus protocol, strictly enforcing majority quorums across odd-numbered clusters, and maintaining MVCC revisions, `etcd` protects cloud infrastructure from data corruption and split-brain disasters.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Node Components Kubelet KubeProxy and CRI](./02-Node-Components-Kubelet-KubeProxy-and-CRI.md) | [Index](../../../README.md) | [04 - Kubernetes API Request Flow Authentication Admission →](./04-Kubernetes-API-Request-Flow-Authentication-Admission.md) |
