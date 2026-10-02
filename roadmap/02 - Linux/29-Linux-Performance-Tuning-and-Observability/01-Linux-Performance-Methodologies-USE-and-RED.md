# 01 — Linux Performance Methodologies: USE and RED

Performance analysis can be overwhelming without a structured framework. Rather than jumping randomly between tools, industry experts follow formal methodologies.

---

## 1. The USE Method (Resource-Oriented)

Developed by Brendan Gregg (Chief Performance Architect), the **USE Method** should be applied to every hardware and software resource (CPUs, Memory, Disks, Network Interfaces, Buses):

For every resource, check:
1. **Utilization:** The percentage of time that the resource was busy or the percentage of capacity used (e.g., Disk %util, RAM % used).
2. **Saturation:** The degree to which extra work is queued waiting for the resource (e.g., CPU run queue length, Disk queue depth).
3. **Errors:** The count of error events (e.g., dropped network packets, disk write errors).

```text
+-------------------+----------------------+--------------------+--------------------+
| Resource          | Utilization          | Saturation         | Errors             |
+-------------------+----------------------+--------------------+--------------------+
| CPU               | mpstat -P ALL (%usr) | uptime (load avg)  | dmesg / mce        |
| Memory            | free -m (used)       | vmstat 1 (si/so)   | dmesg (OOM killer) |
| Disk Storage      | iostat -xz 1 (%util) | iostat 1 (aqu-sz)  | smartctl / dmesg   |
| Network Interface | ip -s link (bytes)   | tc / ifconfig drop | ip -s link (errors)|
+-------------------+----------------------+--------------------+--------------------+
```

---

## 2. The RED Method (Request-Oriented)

Formulated by Tom Wilkie, the **RED Method** applies to services and microservices:
1. **Rate:** Number of requests per second served.
2. **Errors:** Number of failed requests per second.
3. **Duration:** The amount of time requests take (Latency distribution: p50, p95, p99).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (28-Advanced-Storage-LVM-RAID-and-Filesystems)](../28-Advanced-Storage-LVM-RAID-and-Filesystems/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - CPU Profiling and Bottleneck Analysis →](./02-CPU-Profiling-and-Bottleneck-Analysis.md) |
