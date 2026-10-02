# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem 01: The Cascading etcd fsync Outage

### Incident Summary
A financial services Kubernetes cluster hosting 4,000 pods suffered a total control plane crash during morning peak load. All `kubectl` operations timed out, and nodes were marked `NotReady` despite running normally.

### Root Cause
`etcd` data directory (`/var/lib/etcd`) was mounted on standard cloud block storage (EBS gp2) shared with heavy logging workloads.
1. High I/O burst caused disk `wal fsync` latency to spike from 2ms to 240ms.
2. The etcd leader missed its Raft heartbeat window (default 100ms).
3. The remaining members initiated leader elections repeatedly.
4. With no stable leader, `kube-apiserver` health checks failed, causing all API traffic to drop.

### Remediation & Architectural Fix
1. Moved `/var/lib/etcd` to dedicated local NVMe drives with XFS filesystem.
2. Configured ionice priority for the etcd process: `ionice -c2 -n0 -p $(pgrep etcd)`.
3. Set Prometheus alerting on `etcd_disk_wal_fsync_duration_seconds_bucket{le="0.01"}` triggering if fsync exceeds 10ms for > 30 seconds.

---

## Outage Post-Mortem 02: The API Server Memory Bomb via Unindexed List Calls

### Incident Summary
A custom CI/CD automation script polled `kube-apiserver` every 5 seconds executing `kubectl get pods --all-namespaces` without paging.
1. The cluster had 35,000 completed batch pods stored in etcd.
2. Each unindexed `LIST` call required `kube-apiserver` to deserialize and buffer 600MB of JSON in memory.
3. Rapid concurrent executions consumed all 32GB RAM on master nodes, triggering Linux kernel `OOMKilled` on the `kube-apiserver` binary.

### Remediation
1. Enabled **API Priority and Fairness (APF)** to throttle rogue clients.
2. Implemented mandatory chunking (`limit=500`) on all programmatic list calls.
3. Deployed a CronJob to prune completed pods older than 2 hours.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Cloud Controller Manager and Provider Integrations](./06-Cloud-Controller-Manager-and-Provider-Integrations.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
