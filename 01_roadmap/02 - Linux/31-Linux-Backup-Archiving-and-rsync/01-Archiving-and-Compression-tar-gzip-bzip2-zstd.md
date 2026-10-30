# 01 — Archiving and Compression: tar, gzip, and zstd

Archiving bundles multiple files into a single container file. Compression shrinks the data size to save disk space and network bandwidth.

---

## 1. Modern Compression Algorithms Compared

| Algorithm | Tool | Compression Ratio | Speed (Cores) | Best Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **gzip** | `gzip` / `pigz` | Moderate | Moderate (Single or multi) | Standard legacy compatibility |
| **bzip2** | `bzip2` | High | Slow | Deep archival where time is not a factor |
| **xz** | `xz` | Very High | Very Slow | Distro release tarballs, kernel images |
| **zstd** | `zstd` | High | **Blazing Fast** (Multi-core) | **Modern standard** for database backups & cloud images |

---

## 2. Practical `tar` Command Matrix

```bash
# Create gzip-compressed tar archive preserving permissions and numeric owners
tar -czpvf backup-data.tar.gz -C /opt myapp/

# Extract archive to a specific destination folder
tar -xzvf backup-data.tar.gz -C /restored/

# Modern ultra-fast parallel compression using zstd (-I or --zstd)
tar --zstd -cvf database-backup.tar.zst /var/lib/mysql/

# Extract zstd archive
tar --zstd -xvf database-backup.tar.zst -C /data/

# List contents of an archive without extracting
tar -tvf backup-data.tar.gz
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (30-Linux-Log-Management-Logrotate-and-Rsyslog)](../30-Linux-Log-Management-Logrotate-and-Rsyslog/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - rsync Remote Synchronization Deep Dive →](./02-rsync-Remote-Synchronization-Deep-Dive.md) |
