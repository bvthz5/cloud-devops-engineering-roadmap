# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which tool in the daemonless stack is specialized for copying and inspecting remote container images without pulling them?
- [ ] A) Podman
- [ ] B) Buildah
- [x] C) Skopeo
- [ ] D) crun

<details>
<summary><b>Explanation</b></summary>
Skopeo is designed for remote registry operations (copying between registries, inspecting manifests) without requiring a local daemon or downloading layers to disk.
</details>

---

### 2. What modern systemd integration technology in Podman replaces generated shell scripts with `.container` unit files?
- [ ] A) Podlets
- [x] B) Quadlets
- [ ] C) Unitlets
- [ ] D) Cgroups

<details>
<summary><b>Explanation</b></summary>
Podman Quadlets parse declarative <code>.container</code> files and dynamically convert them into native systemd service units.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
