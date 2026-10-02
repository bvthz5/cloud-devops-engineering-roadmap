# 05 - Building a Container from Scratch in Bash/C

## 1. Constructing a Container Without Docker

To demystify containers, we can build a fully isolated container using standard Linux CLI utilities (`unshare`, `cgcreate`, and `chroot`):

```bash
#!/usr/bin/env bash
set -euo pipefail

# 1. Download minimal root filesystem (Alpine Linux)
mkdir -p /tmp/mycontainer/rootfs
cd /tmp/mycontainer
curl -sL http://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-minirootfs-3.19.1-x86_64.tar.gz | tar -xz -C rootfs/

# 2. Setup cgroups v2 resource limits (Max 100MB RAM, 0.5 CPU)
CGROUP_PATH="/sys/fs/cgroup/custom_container"
mkdir -p "$CGROUP_PATH"
echo "100M" > "$CGROUP_PATH/memory.max"
echo "50000 100000" > "$CGROUP_PATH/cpu.max"

# 3. Launch isolated container process
# Spawns isolated PID, Mount, Network, UTS, and IPC namespaces
sudo unshare --pid --mount --uts --ipc --fork /bin/sh -c "
    # Add process to cgroup
    echo \$\$ > $CGROUP_PATH/cgroup.procs

    # Set container hostname
    hostname container-box

    # Mount proc inside isolated mount namespace
    mount -t proc proc /tmp/mycontainer/rootfs/proc

    # Change root filesystem
    chroot /tmp/mycontainer/rootfs /bin/sh
"
```

Inside this shell:
- `hostname` returns `container-box`.
- `ps aux` shows only PID 1!
- Memory usage is physically throttled by the kernel cgroup.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Union Filesystems and OverlayFS Internals](./04-Union-Filesystems-and-OverlayFS-Internals.md) | [Index](../../../README.md) | [06 - Container Security Boundaries and Kernel Surface →](./06-Container-Security-Boundaries-and-Kernel-Surface.md) |
