# 04 - Clustered Database Deployment Patterns: MySQL and Postgres

## 1. Master-Replica Pattern with StatefulSets

In a typical production database topology:
- **`mysql-0`:** Designated Primary (Read-Write).
- **`mysql-1`, `mysql-2`:** Designated Read Replicas (Read-Only).

```text
Write Traffic ──► Service: mysql-primary (selector: app=mysql, role=master) ──► mysql-0
Read Traffic  ──► Service: mysql-read    (selector: app=mysql)              ──► Load balances 0, 1, 2
```

### Initialization Script with Init Container
An `initContainer` checks the Pod ordinal from the hostname (`hostname | awk -F'-' '{print $NF}'`).
- If ordinal is `0`, it initializes the master database configuration (`server-id=100`).
- If ordinal > `0`, it configures the replica to clone from `mysql-0.mysql-headless.default.svc.cluster.local`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Pod Management Policies](./03-OrderedReady-vs-Parallel-Pod-Management-Policies.md) | [README](./README.md) | [05 - DaemonSet Architecture](./05-DaemonSet-Architecture-and-Node-Level-Agents.md) |
