# 11 - Multiple-Choice Assessment (MCQ)

### 1. In which Kubernetes version was Dockershim permanently removed from the Kubelet?
- [ ] A) v1.20
- [ ] B) v1.22
- [x] C) v1.24
- [ ] D) v1.28

<details>
<summary><b>Explanation</b></summary>
Dockershim was deprecated in Kubernetes v1.20 and permanently removed in Kubernetes v1.24.
</details>

---

### 2. Which low-level OCI runtime is written in pure C, offering microsecond startup times and ultra-low memory consumption?
- [ ] A) `runc`
- [ ] B) `runsc`
- [x] C) `crun`
- [ ] D) `kata-runtime`

<details>
<summary><b>Explanation</b></summary>
<code>crun</code> (by Red Hat) is written in C, providing blazing fast container execution and full support for cgroups v2.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
