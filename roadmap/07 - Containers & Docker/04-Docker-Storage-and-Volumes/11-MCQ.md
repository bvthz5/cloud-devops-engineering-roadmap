# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which storage type writes directly to host RAM and vanishes when the container stops?
- [ ] A) Bind Mount
- [x] B) tmpfs Mount
- [ ] C) Named Volume
- [ ] D) OverlayFS

<details>
<summary><b>Explanation</b></summary>
<code>tmpfs</code> mounts store data exclusively in host volatile memory (RAM), never persisting bytes to the underlying storage drive.
</details>

---

### 2. What happens to a file in a read-only lower layer when modified in an overlay2 container?
- [ ] A) It is modified in-place on the base image
- [x] B) It is copied to upperdir before modification (Copy-on-Write)
- [ ] C) The write fails with Read-Only Filesystem error
- [ ] D) A hard link is created in workdir

<details>
<summary><b>Explanation</b></summary>
OverlayFS performs Copy-on-Write (CoW), copying the target file from the read-only lowerdir into the writable upperdir where the modification is applied.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
