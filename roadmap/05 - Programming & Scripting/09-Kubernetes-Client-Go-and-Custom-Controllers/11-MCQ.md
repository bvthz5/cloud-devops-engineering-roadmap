# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which client-go component is responsible for querying the local in-memory cache rather than the kube-apiserver?
- [ ] A) Reflector
- [x] B) Lister
- [ ] C) DeltaFIFO
- [ ] D) DynamicClient

<details>
<summary><b>Explanation</b></summary>
The <code>Lister</code> queries the local <code>Indexer</code> in-memory cache populated by the Informer, eliminating network round-trips to the API server for read queries.
</details>

---

### 2. What happens to a Kubernetes resource when deleted if it contains an item in `metadata.finalizers`?
- [ ] A) It is immediately deleted from etcd
- [ ] B) Deletion fails with HTTP 409 Conflict
- [x] C) It remains in etcd with `deletionTimestamp` set until the finalizer is removed
- [ ] D) The finalizer is automatically stripped after 30 seconds

<details>
<summary><b>Explanation</b></summary>
Kubernetes marks the resource with a <code>deletionTimestamp</code> but prevents physical deletion from etcd until the controller completes cleanup and patches the finalizer out of the array.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
