# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the operational difference between a Named Volume and a Bind Mount?
**Answer**: Named volumes are managed entirely by Docker, isolated in `/var/lib/docker/volumes/`, support volume drivers for remote storage, and decouple container state from host directory structure. Bind mounts map an arbitrary host directory directly into the container, exposing the container to host permission issues and path differences across platforms.

### Q2: How does the `overlay2` storage driver handle file modifications?
**Answer**: `overlay2` uses Copy-on-Write (CoW). When a file in a read-only lower layer is modified, the kernel copies the entire file into the container's writable `upperdir` before executing the write. The original image layer file remains completely unchanged.

### Q3: Why should production database volumes use Named Volumes rather than writing to the container layer?
**Answer**: Writing to the container writable layer routes through the OverlayFS driver, incurring heavy CPU and I/O write amplification penalties. Named volumes bypass the union filesystem entirely, writing directly to the host filesystem at native disk I/O speeds.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
