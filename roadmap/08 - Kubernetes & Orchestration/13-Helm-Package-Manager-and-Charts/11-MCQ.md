# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Where does Helm v3 store release state information?
- [ ] A) In the Tiller pod memory
- [x] B) In Kubernetes Secrets in the release namespace
- [ ] C) In a local SQLite file on the client machine
- [ ] D) In a dedicated etcd bucket

<details>
<summary>Explanation</summary>
Helm v3 stores release metadata and revisions as encrypted/base64-encoded Kubernetes Secrets directly inside the namespace where the application is deployed.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
