# 01 - Kubernetes Disaster Recovery Strategy: RTO and RPO

## 1. The Two Pillars of Disaster Recovery

- **RPO (Recovery Point Objective):** Maximum acceptable data loss measured in time (e.g. "We can lose up to 15 minutes of transactional data").
- **RTO (Recovery Time Objective):** Maximum acceptable downtime to bring the cluster back to full operation (e.g. "Cluster must be restored within 30 minutes").

```text
[ Data Mutation Event ] ──────► [ Incident / Disaster ] ──────► [ Fully Restored ]
         │                              │                               │
         └─────────── RPO ──────────────┘─────────────── RTO ───────────┘
               (Data Loss Window)              (Downtime / Recovery Duration)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (18-Gateway-API-and-Modern-Traffic-Routing)](../18-Gateway-API-and-Modern-Traffic-Routing/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Etcd Backup Snapshot Save and Disaster Restore →](./02-Etcd-Backup-Snapshot-Save-and-Disaster-Restore.md) |
