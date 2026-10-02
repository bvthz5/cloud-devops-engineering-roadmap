# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Why does setting `immutable: true` on large ConfigMaps improve cluster performance?
- [ ] A) It compresses the configuration into gzip
- [x] B) It allows kubelet to terminate watch loops on kube-apiserver
- [ ] C) It caches the data in etcd memory
- [ ] D) It prevents pods from accessing the file

<details>
<summary>Explanation</summary>
Setting `immutable: true` instructs Kubelet that it does not need to poll or watch the API server for changes to this resource, significantly reducing control plane overhead at scale.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
