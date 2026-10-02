# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The StatefulSet Split-Brain Disaster

### Incident Summary
A 3-node ZooKeeper / Kafka cluster running in a multi-AZ cloud setup experienced a 15-second network partition between Availability Zone 1 and Availability Zone 2.
Because Pod `zookeeper-0` was in AZ 1 and `zookeeper-1` / `zookeeper-2` were in AZ 2, two leaders were elected simultaneously, corrupting Kafka message offsets and requiring an 8-hour disaster restore.

### Root Cause
Improper quorum configuration and aggressive node eviction timers caused `kube-controller-manager` to declare `zookeeper-0` dead and attempt to schedule a replacement while the real node was still running.

### Remediation
1. Configured ZooKeeper quorum with an odd number of voting members ($2F + 1$).
2. Deployed Pod Topology Spread Constraints across 3 distinct Availability Zones.
3. Configured fencing via `ReadWriteOncePod` CSI volume access modes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - DaemonSet Updates](./06-DaemonSet-Update-Strategies-and-HostPort-Considerations.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
