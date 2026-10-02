# 01 - Storage Drivers and Overlay2 Deep Dive

## 1. The Overlay2 Architecture

Docker utilizes the Linux kernel's **OverlayFS** union filesystem driver (`overlay2`) to layer images and container read-write layers:

```text
/var/lib/docker/overlay2/<cache-id>/
├── merged/    <-- The active rootfs seen by the running container
├── upper/     <-- Container read-write layer (Stores modified/new files)
├── work/      <-- Atomic rename scratch space for kernel
└── lower/     <-- Read-only base layers (Symlinked layer tree)
```

### Inode Consumption Warning
Because `overlay2` creates directories and hard links for every layer, running thousands of small containers can exhaust the host filesystem's **Inodes** long before disk space runs out! Always monitor:
```bash
df -i /var/lib/docker
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Dockerfile-Best-Practices-and-Multi-Stage)](../03-Dockerfile-Best-Practices-and-Multi-Stage/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Named Volumes vs Bind Mounts vs tmpfs →](./02-Named-Volumes-vs-Bind-Mounts-vs-tmpfs.md) |
