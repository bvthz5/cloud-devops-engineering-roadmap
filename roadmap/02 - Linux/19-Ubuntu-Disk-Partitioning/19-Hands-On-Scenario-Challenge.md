# 19. Practical Scenario Challenge

## Scenario
You are assigned to configure a database server storage.
1. Create a 30GB LVM Logical Volume `lv_db` inside Volume Group `vg_data`.
2. Format as `XFS` filesystem.
3. Mount permanently at `/var/lib/mysql` using UUID.
4. Mount with `noatime,nodev` options.
5. Extend the partition online by +10GB.

## Validation Commands
```bash
sudo lvs
sudo xfs_growfs /var/lib/mysql
findmnt /var/lib/mysql
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - MCQs and Quick Revision](./18-MCQs-and-Quick-Revision.md) | [README](./README.md) | [20 - Quick Revision Notes](./20-Quick-Revision-Notes.md) |
