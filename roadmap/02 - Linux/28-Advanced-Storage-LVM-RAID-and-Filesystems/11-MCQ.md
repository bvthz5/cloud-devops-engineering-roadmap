# 11 — Multiple Choice Questions: Storage & LVM

---

### Q1. Which command expands an XFS filesystem online?
- [ ] A) `resize2fs /dev/vg/lv`
- [ ] B) `xfs_growfs /mnt/data`
- [ ] C) `xfs_expand /dev/vg/lv`
- [ ] D) `fsck.xfs -E /mnt/data`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
<code>xfs_growfs</code> requires the target to be the active <b>mount point directory</b>, unlike <code>resize2fs</code> which accepts the block device path.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
