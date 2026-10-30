# 05 - External Volume Plugins and Cloud Storage

## 1. Shared Storage via Volume Plugins

In distributed multi-node clusters, standard local volumes cannot follow containers when rescheduled on another host. **Docker Volume Plugins** connect Docker to remote distributed storage (NFS, AWS EFS, Ceph):

```bash
# Mount remote AWS EFS or NFS directly as a Docker named volume
docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=10.0.1.50,rw,nfsvers=4 \
  --opt device=:/data \
  shared_nfs_volume

# Containers on any host can mount the shared data!
docker run -d -v shared_nfs_volume:/app/shared webapp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Tmpfs Mounts and In Memory Security](./04-Tmpfs-Mounts-and-In-Memory-Security.md) | [Index](../../../README.md) | [06 - Storage Optimization and Dangling Cleanup →](./06-Storage-Optimization-and-Dangling-Cleanup.md) |
