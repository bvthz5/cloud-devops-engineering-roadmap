# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The PLEG Cascade Node Meltdown

### Incident Summary
A cluster node running 120 pods suddenly transitioned to `NotReady`. As Kubelet evicted workloads to other nodes, those nodes also became `NotReady`, cascading across all 15 nodes in the pool.

### Root Cause
Kubelet's **Pod Lifecycle Event Generator (PLEG)** relies on `containerd` to report container states. A rogue Java application was logging 80,000 lines per second directly to stdout, locking containerd's FIFO pipes and causing containerd to hang. PLEG missed its 3-minute heartbeat, and the node controller declared it dead.

### Remediation
1. Configured container log limits in `/etc/docker/daemon.json` or `/etc/containerd/config.toml` (`max-size: 50m`, `max-file: 3`).
2. Configured log rotation with non-blocking logging mode.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Audit & Forensics](./06-Cluster-Auditing-Event-Analysis-and-Forensics.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
