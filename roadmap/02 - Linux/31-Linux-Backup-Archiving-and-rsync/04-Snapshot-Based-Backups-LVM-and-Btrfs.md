# 04 — Snapshot-Based Backups (LVM and Cloud)

Backing up active databases (PostgreSQL, MySQL) by copying live files directly results in corrupted data blocks due to mid-write collisions. Taking an atomic **point-in-time snapshot** captures a frozen, crash-consistent image in milliseconds.

---

## 1. LVM Snapshot Lifecycle

```bash
# 1. Briefly flush database tables and acquire read lock (via SQL script)
# FLUSH TABLES WITH READ LOCK;

# 2. Create a 5GB LVM snapshot volume (executes in 200ms!)
sudo lvcreate -L 5G -s -n lv_db_snap /dev/vg_production/lv_database

# 3. Release database lock (database resumes serving production writes immediately!)
# UNLOCK TABLES;

# 4. Mount the frozen snapshot volume read-only
sudo mkdir -p /mnt/snapshot
sudo mount -o ro /dev/vg_production/lv_db_snap /mnt/snapshot

# 5. Backup the frozen filesystem safely using tar or rsync
tar --zstd -cvf /backups/db-frozen.tar.zst -C /mnt/snapshot .

# 6. Unmount and delete the snapshot volume (frees copy-on-write storage)
sudo umount /mnt/snapshot
sudo lvremove -y /dev/vg_production/lv_db_snap
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Automated Backups over SSH](./03-Automated-Backups-over-SSH.md) | [README](./README.md) | [05 - Disaster Recovery RPO & RTO](./05-Disaster-Recovery-Strategies-RPO-and-RTO.md) |
