# 09 - Interview Questions & Architectural Scenarios

### Q1: Why does a StatefulSet require a Headless Service?
**Answer:**
A StatefulSet requires a Headless Service (`clusterIP: None`) to give each Pod an individual, resolvable DNS A record (`<pod-name>.<service-name>.<namespace>.svc.cluster.local`). Without a Headless Service, CoreDNS would only return a single virtual ClusterIP, making peer-to-peer clustering and direct primary-replica discovery impossible.

---

### Q2: What happens to a StatefulSet's persistent storage when you scale replicas down from 3 to 1?
**Answer:**
The two terminated pods (`pod-2` and `pod-1`) are deleted, but **their PersistentVolumeClaims (`data-pod-2` and `data-pod-1`) are intentionally retained**. They are never automatically purged. If the StatefulSet is scaled back up to 3 replicas later, the newly created pods will re-attach to the existing disks with all historical data intact.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
