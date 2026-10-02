# 05 - DaemonSet Architecture and Node-Level Agents

## 1. What Is a DaemonSet?

A **DaemonSet** ensures that all (or some) worker nodes run exactly **one copy of a Pod**. As new nodes are added to the cluster, the DaemonSet controller automatically adds pods to them.

```text
+-------------------------------------------------------------------------------+
|                             KUBERNETES CLUSTER                                |
|                                                                               |
|  +------------------------+  +------------------------+  +-----------------+  |
|  | Node 01                |  | Node 02                |  | Node 03         |  |
|  | [Fluentbit Log Agent]  |  | [Fluentbit Log Agent]  |  | [Fluentbit...]  |  |
|  | [Prometheus Exporter]  |  | [Prometheus Exporter]  |  | [Prometheus...] |  |
|  | [Cilium CNI Router]    |  | [Cilium CNI Router]    |  | [Cilium CNI...] |  |
|  +------------------------+  +------------------------+  +-----------------+  |
+-------------------------------------------------------------------------------+
```

---

## 2. Common DaemonSet Use Cases

1. **Log Collection Daemons:** `fluentd`, `promtail`, `fluentbit` reading `/var/log/containers`.
2. **Node Monitoring Exporters:** `node-exporter` exposing hardware metrics.
3. **Cluster Networking (CNI):** `calico-node`, `cilium` running kernel routing agents.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Clustered Database Deployment Patterns MySQL Postgres](./04-Clustered-Database-Deployment-Patterns-MySQL-Postgres.md) | [Index](../../../README.md) | [06 - DaemonSet Update Strategies and HostPort Considerations →](./06-DaemonSet-Update-Strategies-and-HostPort-Considerations.md) |
