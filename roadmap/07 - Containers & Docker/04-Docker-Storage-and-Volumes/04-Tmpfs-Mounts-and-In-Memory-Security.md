# 04 - Tmpfs Mounts and In-Memory Security

## 1. Zero-Disk Trace Security

Writing sensitive authentication tokens, TLS private keys, or decrypted secrets to a container's writable layer risks persistence in host swap files or image layer caches.

**`tmpfs` mounts**:
1. Mount directly into host volatile RAM.
2. Disappear immediately when the container stops.
3. Prevent sensitive data from ever touching physical SSD/HDD media.

```bash
docker run -d \
  --name auth-service \
  --mount type=tmpfs,target=/app/keys,tmpfs-size=32m,tmpfs-mode=0700 \
  my-auth-image
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Volume Lifecycle & Backup](./03-Volume-Lifecycle-Backup-and-Restoration.md) | [README](./README.md) | [05 - External Volume Plugins](./05-External-Volume-Plugins-and-Cloud-Storage.md) |
