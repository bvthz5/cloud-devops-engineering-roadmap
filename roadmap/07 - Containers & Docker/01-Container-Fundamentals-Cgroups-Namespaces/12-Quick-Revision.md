# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Inspect process namespaces
ls -la /proc/$$/ns/

# Attach to container namespace with host tools
sudo nsenter -t <PID> -n -m /bin/bash

# cgroups v2 paths
/sys/fs/cgroup/
├── cpu.max         # e.g., 100000 100000 (1 core)
├── memory.max      # e.g., 512M
└── pids.max        # e.g., 200

# The 7 Namespaces:
# PID (Processes), NET (Networking), MNT (Mounts), IPC (Shared Memory),
# UTS (Hostname), USER (UID/GID), CGROUP (Cgroup root)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (02-Docker-Architecture-and-CLI) →](../02-Docker-Architecture-and-CLI/01-Docker-Engine-Architecture-and-Subsystems.md) |
