# Module 29: Linux Performance Tuning and Observability

Performance optimization and observability are defining skills for Senior DevOps and Site Reliability Engineers. When a production microservice experiences latency spikes, databases slow down, or CPU saturates at 100%, engineers must systematically pinpoint the bottleneck—CPU, memory, disk I/O, or network—without guessing.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Apply Brendan Gregg's **USE Method** (Utilization, Saturation, Errors) for systematic diagnostics.
- Differentiate between CPU user time, system time, and I/O wait (`%iowait`).
- Master Linux memory management: PageCache, anonymous memory, Swappiness, and OOM-killer tuning.
- Identify disk I/O latency bottlenecks using `iostat -xz 1` (`%util`, `await`).
- Extract historical system performance metrics during postmortems using `sar` (sysstat).
- Tune critical Linux kernel parameters using `sysctl` and file descriptor limits (`limits.conf`).
- Execute the famous **60-Second Linux Performance Triage Runbook**.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Performance Methodologies (USE & RED)](./01-Linux-Performance-Methodologies-USE-and-RED.md) | Brendan Gregg's USE Method, RED method, and resource checklists | ✅ Complete |
| 02 | [CPU Profiling & Bottleneck Analysis](./02-CPU-Profiling-and-Bottleneck-Analysis.md) | Load averages, user vs sys vs iowait, `top`, `mpstat`, and `pidstat` | ✅ Complete |
| 03 | [Memory Tuning: Swap, Cache & OOM](./03-Memory-Tuning-Swap-PageCache-and-OOM.md) | PageCache, dirty pages, `vm.swappiness`, and OOM killer scores | ✅ Complete |
| 04 | [Disk I/O Analysis & Storage Bottlenecks](./04-Disk-IO-Analysis-and-Storage-Bottlenecks.md) | `iostat -xz 1`, `%util`, queue depth, IOPS, and I/O schedulers | ✅ Complete |
| 05 | [Historical Monitoring with sysstat / sar](./05-Historical-System-Activity-Monitoring-sysstat-sar.md) | Reconstructing past incidents, `/var/log/sa/`, and trend analysis | ✅ Complete |
| 06 | [Kernel Tuning with sysctl & Limits](./06-Linux-Kernel-Tuning-with-sysctl.md) | `/etc/sysctl.d/`, `limits.conf`, file descriptors, and socket buffers | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | High load with 0% CPU (D-state hang), memory leaks, and disk queues | ✅ Complete |
| 08 | [Troubleshooting Guide: 60-Second Triage](./08-Troubleshooting.md) | Brendan Gregg's 60-second first-response CLI triage runbook | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical performance interview questions for SRE/DevOps roles | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Generate simulated CPU/IO stress with `stress-ng` and profile bottlenecks | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | High-density command matrix, metric definitions, and sysctl tunables | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 28: Storage & LVM](../28-Advanced-Storage-LVM-RAID-and-Filesystems/README.md) | [Linux Roadmap Index](../README.md) | [01 - Performance Methodologies](./01-Linux-Performance-Methodologies-USE-and-RED.md) |
