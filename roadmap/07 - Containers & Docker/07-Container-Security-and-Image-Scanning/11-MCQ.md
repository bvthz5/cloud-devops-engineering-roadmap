# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which flag instructs Docker to prevent container processes from acquiring additional privileges via setuid binaries?
- [ ] A) `--secure`
- [ ] B) `--disable-root`
- [x] C) `--security-opt no-new-privileges:true`
- [ ] D) `--cap-drop=SUID`

<details>
<summary><b>Explanation</b></summary>
<code>--security-opt no-new-privileges:true</code> sets the PR_SET_NO_NEW_PRIVS bit in the Linux kernel, preventing setuid binaries from escalating privileges.
</details>

---

### 2. What tool is widely used in CI/CD to scan container images for CVE vulnerabilities in OS packages and lockfiles?
- [ ] A) Valgrind
- [x] B) Trivy
- [ ] C) Wireshark
- [ ] D) Docker Swarm

<details>
<summary><b>Explanation</b></summary>
Trivy (by Aqua Security) is the industry standard open-source vulnerability scanner for container images, Git repos, and Kubernetes clusters.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
