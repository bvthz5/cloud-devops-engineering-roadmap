# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between PersistentVolume (PV) and PersistentVolumeClaim (PVC)?
**Answer:**
A **PersistentVolume (PV)** is a physical storage asset provisioned in the cluster (e.g. a 100GB AWS EBS volume). It is cluster-scoped and managed by cluster administrators.
A **PersistentVolumeClaim (PVC)** is a developer's request for storage specifying capacity, access mode, and storage class. It is namespace-scoped. The PVC acts like a voucher that binds to a matching PV.

---

### Q2: What happens if a developer deletes a PVC whose PV has a `reclaimPolicy: Retain`?
**Answer:**
The PVC is deleted, but the backing PV is **not destroyed**. The PV transitions to the `Released` status. The data on the underlying disk remains intact, but the PV cannot be bound to another PVC until an administrator manually cleans the storage and resets the `spec.claimRef`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
