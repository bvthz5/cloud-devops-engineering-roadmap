# 03 - Volume Lifecycle, Backup, and Restoration

## 1. The Ephemeral Backup Container Pattern

Because named volumes reside inside `/var/lib/docker/volumes/`, the industry standard pattern for taking backups is spawning an ephemeral helper container that mounts the target volume and streams a compressed archive to the host:

```bash
# Backup: Archive 'pg_data' volume into current directory as backup.tar.gz
docker run --rm \
  -v pg_data:/volume:ro \
  -v "$(pwd)":/backup \
  alpine tar -czf /backup/backup.tar.gz -C /volume .

# Restore: Extract backup.tar.gz into a fresh volume 'pg_data_restored'
docker volume create pg_data_restored
docker run --rm \
  -v pg_data_restored:/volume \
  -v "$(pwd)":/backup:ro \
  alpine tar -xzf /backup/backup.tar.gz -C /volume
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Named Volumes vs Bind Mounts vs tmpfs](./02-Named-Volumes-vs-Bind-Mounts-vs-tmpfs.md) | [Index](../../../README.md) | [04 - Tmpfs Mounts and In Memory Security →](./04-Tmpfs-Mounts-and-In-Memory-Security.md) |
