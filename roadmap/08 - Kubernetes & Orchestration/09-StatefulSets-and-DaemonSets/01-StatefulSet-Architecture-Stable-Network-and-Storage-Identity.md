# 01 - StatefulSet Architecture: Stable Network and Storage Identity

## 1. Why Not Deployments for Databases?

In a Deployment, Pods are interchangeable and fungible. They have random names (`web-7d648b9c8-k4z9l`), share storage volumes, and terminate in arbitrary order.
Stateful distributed systems (e.g. PostgreSQL, Kafka, Elasticsearch, Cassandra, ZooKeeper) require:
1. **Stable, Unique Network Identifiers:** Persistent DNS hostnames across restarts.
2. **Stable, Dedicated Persistent Storage:** Each replica must bind to its own specific disk.
3. **Ordered Deployment and Scaling:** Node 0 must start before Node 1; Node 1 must terminate before Node 0.

```text
+-------------------------------------------------------------------------------+
|                       STATEFULSET: "kafka" (Replicas: 3)                      |
+-------------------+-------------------+-------------------+-------------------+
| Pod: kafka-0      | Pod: kafka-1      | Pod: kafka-2      |
| IP: 10.244.1.5    | IP: 10.244.2.8    | IP: 10.244.3.11   |
| PVC: data-kafka-0 | PVC: data-kafka-1 | PVC: data-kafka-2 |
| PV: 100GB NVMe    | PV: 100GB NVMe    | PV: 100GB NVMe    |
+-------------------+-------------------+-------------------+
```

---

## 2. Ordinal Indexing

For a StatefulSet with $N$ replicas, each Pod is assigned an integer ordinal index from `0` to $N-1$:
- Name: `<statefulset-name>-<ordinal>` (e.g. `redis-0`, `redis-1`, `redis-2`).
- Predictable DNS: `redis-0.redis-svc.default.svc.cluster.local`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - VolumeClaimTemplates](./02-VolumeClaimTemplates-and-Per-Replica-Storage.md) |
