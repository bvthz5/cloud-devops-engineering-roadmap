# 09 - Interview Questions & Architectural Scenarios

### Q1: What does container Exit Code 137 indicate, and how do you verify its root cause?
**Answer**: Exit Code 137 indicates termination by `SIGKILL` ($128 + 9 = 137$). The two most common causes are: 1) The Linux kernel OOM Killer terminating the process after exceeding its `memory.max` cgroup limit, 2) A user or orchestrator issuing `docker kill` or timing out during `docker stop`. It is verified by inspecting `dmesg` for "Memory cgroup out of memory" or checking `docker inspect` for `"OOMKilled": true`.

### Q2: Why does setting a CPU limit cause high application latency even when average CPU usage is low?
**Answer**: Linux CFS (Completely Fair Scheduler) enforces CPU limits across rigid 100ms periods. If a bursty multi-threaded application consumes its entire period quota in the first 15ms, the kernel pauses all threads for the remaining 85ms of the period, introducing artificial latency spikes despite low aggregate CPU metrics.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
