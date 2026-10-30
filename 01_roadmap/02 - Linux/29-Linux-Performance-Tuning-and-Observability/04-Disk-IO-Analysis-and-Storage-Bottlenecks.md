# 04 — Disk I/O Analysis and Storage Bottlenecks

Storage I/O latency is orders of magnitude slower than CPU or RAM. A slow disk will cause application threads to block in `D` state, spiking load average and grinding services to a halt.

---

## 1. Deep Dive: `iostat -xz 1`

```bash
iostat -xz 1
```
**Key Metrics to Monitor:**
- **`r/s` and `w/s`:** Read and write IOPS (I/O requests per second).
- **`rMB/s` and `wMB/s`:** Throughput in Megabytes per second.
- **`await`:** The average time (in milliseconds) that I/O requests spent waiting in queue plus being serviced by the hardware.
  - On enterprise NVMe SSDs: `< 1.0 ms`.
  - On cloud SSDs (EBS gp3): `< 5.0 ms`.
  - **If `await > 20 ms`, storage is saturated!**
- **`aqu-sz` (Average Queue Size):** Number of I/O requests queued waiting for service.
- **`%util`:** Percentage of time during which I/O requests were issued to the device. If `%util` approaches 100%, the storage device is fully saturated.

---

## 2. Identifying Which Process is Saturating Disk (`iotop`)

```bash
# Live display of top I/O consumers (processes doing heavy disk writes/reads)
sudo iotop -oPa
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Memory Tuning Swap PageCache and OOM](./03-Memory-Tuning-Swap-PageCache-and-OOM.md) | [Index](../../../README.md) | [05 - Historical System Activity Monitoring sysstat sar →](./05-Historical-System-Activity-Monitoring-sysstat-sar.md) |
