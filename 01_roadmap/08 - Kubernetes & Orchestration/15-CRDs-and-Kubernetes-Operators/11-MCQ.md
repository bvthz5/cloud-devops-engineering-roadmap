# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What mechanism prevents a Kubernetes object from being deleted from etcd until a controller performs clean up?
- [ ] A) ResourceQuota
- [x] B) Finalizers
- [ ] C) MutatingAdmissionWebhook
- [ ] D) PodSecurityStandards

<details>
<summary>Explanation</summary>
When an object has `metadata.finalizers` populated, the API server blocks hard deletion and sets `deletionTimestamp`, waiting for the controller to execute cleanup and remove the finalizer string.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
