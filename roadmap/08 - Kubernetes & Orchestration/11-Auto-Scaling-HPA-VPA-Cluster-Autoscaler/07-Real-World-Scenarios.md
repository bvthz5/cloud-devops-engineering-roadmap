# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Cloud API Rate-Limit Autoscaling Deadlock

### Incident Summary
During Black Friday, a major retail platform experienced a sudden 10x traffic spike. Hundreds of pods were marked `Pending`, but no new worker nodes were added for 22 minutes, leading to widespread downtime.

### Root Cause
1. 15 different microservice HPAs scaled simultaneously, triggering the Cluster Autoscaler.
2. The Cluster Autoscaler flooded the AWS EC2 API with thousands of `DescribeLaunchConfigurations` calls.
3. AWS enforced strict API throttling (`RequestLimitExceeded`).
4. The Autoscaler backed off exponentially, failing to provision new EC2 instances.

### Remediation
1. Switched from Cluster Autoscaler to **Karpenter**, which bundles instance requests into batch fleet API calls.
2. Pre-scaled node pools 2 hours before planned marketing promotions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Anti-Patterns & Flapping](./06-Autoscaling-Anti-Patterns-Thrashing-and-Conflict-Resolution.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
