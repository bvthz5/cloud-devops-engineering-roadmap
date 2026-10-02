# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
How is a Headless Service created in Kubernetes?
- [ ] A) Setting `spec.type: None`
- [x] B) Setting `spec.clusterIP: "None"`
- [ ] C) Setting `metadata.annotations: headless=true`
- [ ] D) Omitting `spec.ports`

<details>
<summary>Explanation</summary>
Setting `spec.clusterIP: "None"` tells Kubernetes not to allocate a virtual IP, causing CoreDNS to return direct A records for all backing pods.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
