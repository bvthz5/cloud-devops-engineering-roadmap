# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which daemon configuration parameter allows containers to remain running during Docker daemon updates?
- [ ] A) `keep-alive`
- [x] B) `live-restore`
- [ ] C) `persist-containers`
- [ ] D) `hot-reload`

<details>
<summary><b>Explanation</b></summary>
<code>live-restore: true</code> instructs containerd-shim to maintain container execution when the <code>dockerd</code> daemon restarts.
</details>

---

### 2. What exit code is returned when a container is killed by the Linux kernel OOM Killer?
- [ ] A) Exit Code 1
- [ ] B) Exit Code 0
- [x] C) Exit Code 137
- [ ] D) Exit Code 143

<details>
<summary><b>Explanation</b></summary>
128 + 9 (SIGKILL) = 137, indicating the process was killed by the kernel OOM Killer.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
