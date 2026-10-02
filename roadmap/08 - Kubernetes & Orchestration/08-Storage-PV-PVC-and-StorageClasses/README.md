# 08 - Storage, PV, PVC, and StorageClasses

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Storage-Architecture-PV-and-PVC-Binding.md` — The persistent storage decoupling: PersistentVolumes (PV) vs PersistentVolumeClaims (PVC) and binding phases.
2. `02-StorageClasses-and-Dynamic-Provisioning.md` — JIT volume provisioning: provisioners, parameters, `volumeBindingMode: WaitForFirstConsumer`, and default classes.
3. `03-Container-Storage-Interface-CSI-Architecture.md` — The storage plugin standard: CSI node plugins, controller plugins, and RPC interfaces.
4. `04-Access-Modes-ReadWriteOnce-ReadWriteMany-and-Block-Volumes.md` — Access modes: `ReadWriteOnce` (RWO), `ReadOnlyMany` (ROX), `ReadWriteMany` (RWX), `ReadWriteOncePod` (RWOP), and raw block storage.
5. `05-Volume-Expansion-and-Reclaim-Policies.md` — Expanding disks online (`allowVolumeExpansion`), and lifecycle retention policies (`Retain`, `Delete`).
6. `06-VolumeSnapshots-and-Stateful-Backup-Workflows.md` — Point-in-time snapshots: `VolumeSnapshotClass`, `VolumeSnapshot`, and restoring data to new PVCs.
7. `07-Real-World-Scenarios.md` — Production post-mortems: multi-attach errors during node drain, and out-of-space database crashes.
8. `08-Troubleshooting.md` — Diagnostic decision tree for `Pending` PVCs and `FailedMount` events.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes storage.
10. `10-Hands-On-Practice.md` — Production lab: dynamic volume provisioning, online expansion, and volume snapshot restore.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density StorageClass and PV reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 07 - ConfigMaps & Secrets](../07-ConfigMaps-and-Secrets/README.md) | [README](./README.md) | [01 - Storage Architecture](./01-Kubernetes-Storage-Architecture-PV-and-PVC-Binding.md) |
