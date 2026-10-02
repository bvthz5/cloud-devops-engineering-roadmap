# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
If a container exceeds its defined memory limit (`resources.limits.memory`), what immediately happens?
- [ ] A) CPU throttling is applied
- [x] B) The kernel OOM-killer sends SIGKILL and the container terminates with exit code 137
- [ ] C) Kubelet temporarily increases the limit
- [ ] D) The Pod is moved to the Pending state

<details>
<summary>Explanation</summary>
Memory is an incompressible resource. When memory consumption breaches the cgroup boundary, the Linux kernel invokes the OOM killer, resulting in exit code 137 (128 + 9 [SIGKILL]).
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
