# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Cloud Multi-Attach Deadlock

### Incident Summary
During a planned cluster upgrade, worker node A was drained. The production MySQL pod was terminated and scheduled to worker node B, but remained stuck in `ContainerCreating` for 45 minutes.

### Root Cause
1. AWS EBS volumes only support `ReadWriteOnce` attachment to a single EC2 instance at a time.
2. Worker node A crashed before cleanly detaching the EBS volume via AWS API.
3. The Kubernetes `attachdetach-controller` refused to attach the volume to worker node B because AWS reported the volume was still locked to node A (`Multi-Attach error for volume "vol-0a1b2c3d"`).

### Remediation
1. Implemented **Non-Graceful Node Shutdown** controller (k8s 1.28+) using node out-of-service taints.
2. Decreased volume unmount timeout settings in cloud provider CSI controllers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - VolumeSnapshots and Stateful Backup Workflows](./06-VolumeSnapshots-and-Stateful-Backup-Workflows.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
