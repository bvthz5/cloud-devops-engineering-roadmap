# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Diagnosing OOMKilled via Kernel Logs

When a container exits with 137, confirm whether it was OOMKilled by inspecting kernel ring buffers:

```bash
# Search dmesg for kernel OOM invocations
sudo dmesg -T | grep -i "oom-killer" -A 10
# Output:
# [Fri Oct  2 09:30:15 2026] Memory cgroup out of memory: Killed process 54321 (java)
# total-vm:2048500kB, anon-rss:1048576kB, file-rss:0kB, shmem-rss:0kB
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
