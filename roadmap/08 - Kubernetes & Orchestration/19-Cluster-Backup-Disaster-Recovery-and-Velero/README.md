# 19 - Cluster Backup, Disaster Recovery, and Velero

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Disaster-Recovery-Strategy-RTO-and-RPO.md` — Enterprise DR planning: Recovery Time Objective (RTO), Recovery Point Objective (RPO), and failure modes.
2. `02-Etcd-Backup-Snapshot-Save-and-Disaster-Restore.md` — Control plane disaster recovery: `etcdctl snapshot save`, TLS flags, restoring to an alternate directory, and static pod restart.
3. `03-Velero-Architecture-and-CSI-VolumeSnapshot-Integration.md` — Application-aware backups: Velero server, custom plugins, object storage targets (S3/GCS), and CSI VolumeSnapshot integration.
4. `04-Scheduled-Automated-Backups-and-Object-Storage-Targets.md` — Continuous backup policies: `Schedule` CRD, include/exclude namespaces, and retention TTLs.
5. `05-Cross-Cluster-Migration-and-Cluster-Rebuilding.md` — Lift-and-shift migration: backing up from on-prem/dev cluster and restoring to cloud production cluster.
6. `06-DR-Testing-Validation-and-RTO-Benchmarking.md` — Chaos simulation: testing cold-standby restoration, validating PVC attachments, and RTO benchmarking.
7. `07-Real-World-Scenarios.md` — Production post-mortems: accidental cluster deletion recovery via Velero, and corrupted etcd snapshot restore failure.
8. `08-Troubleshooting.md` — Diagnostic runbook for Velero `PartiallyFailed` backups, S3 IAM permission issues, and CSI snapshot timeouts.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes disaster recovery and etcd maintenance.
10. `10-Hands-On-Practice.md` — Production lab: taking a live etcd snapshot, simulating data corruption, and restoring cluster state.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density etcdctl and Velero disaster recovery command cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 18 - Gateway API](../18-Gateway-API-and-Modern-Traffic-Routing/README.md) | [README](./README.md) | [01 - DR Strategy](./01-Kubernetes-Disaster-Recovery-Strategy-RTO-and-RPO.md) |
