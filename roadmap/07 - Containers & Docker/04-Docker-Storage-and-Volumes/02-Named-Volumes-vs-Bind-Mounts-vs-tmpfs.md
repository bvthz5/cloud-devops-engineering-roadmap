# 02 - Named Volumes vs Bind Mounts vs tmpfs

## 1. Storage Type Comparison

```text
+-----------------------------------------------------------------------------------+
|                        Docker Storage Type Comparison                             |
+-------------------+-----------------------+---------------------------------------+
| Type              | Host Location         | Primary Use Case                      |
+-------------------+-----------------------+---------------------------------------+
| **Named Volume**  | `/var/lib/docker/     | Production databases (PostgreSQL,     |
|                   | volumes/<name>/_data` | MySQL). Managed entirely by Docker.   |
+-------------------+-----------------------+---------------------------------------+
| **Bind Mount**    | Arbitrary host path   | Local development (live code reload), |
|                   | (e.g. `/home/user/app`)| mounting host configuration files.    |
+-------------------+-----------------------+---------------------------------------+
| **tmpfs Mount**   | Host RAM memory only  | Ephemeral secrets, session keys,      |
|                   | (Never written to disk)| high-speed temporary scratch buffers. |
+-------------------+-----------------------+---------------------------------------+
```

---

## 2. Modern Syntax: `-v` vs `--mount`

Docker recommends the explicit, typed **`--mount`** flag over legacy `-v`:

```bash
# 1. Named Volume with --mount
docker run -d \
  --name db \
  --mount type=volume,source=pg_data,target=/var/lib/postgresql/data \
  postgres:16-alpine

# 2. Read-Only Bind Mount with --mount
docker run -d \
  --name web \
  --mount type=bind,source=/etc/nginx/nginx.conf,target=/etc/nginx/nginx.conf,readonly \
  nginx:alpine

# 3. Tmpfs Mount with memory limit (64MB)
docker run -d \
  --name secure_app \
  --mount type=tmpfs,target=/tmp/secrets,tmpfs-size=67108864,tmpfs-mode=1770 \
  myapp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Storage Drivers and Overlay2 Deep Dive](./01-Storage-Drivers-and-Overlay2-Deep-Dive.md) | [Index](../../../README.md) | [03 - Volume Lifecycle Backup and Restoration →](./03-Volume-Lifecycle-Backup-and-Restoration.md) |
