# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Exit Code Reference:
# 0   -> Success
# 1   -> Application Crash
# 126 -> Permission Denied (Not executable)
# 127 -> Command Not Found
# 137 -> OOMKilled / SIGKILL (128 + 9)
# 139 -> Segmentation Fault (128 + 11)
# 143 -> SIGTERM (128 + 15)

# Inspect cgroups v2 metrics directly
cat /sys/fs/cgroup/system.slice/docker-<id>.scope/cpu.stat
cat /sys/fs/cgroup/system.slice/docker-<id>.scope/memory.current

# Attach host tools via nsenter
sudo nsenter -t <PID> -n -m /bin/sh
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [01 - Container Fundamentals](../01-Container-Fundamentals-Cgroups-Namespaces/README.md) |
