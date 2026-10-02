# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Overlapping CronJob Node Exhaustion

### Incident Summary
A cluster running 50 nodes crashed on Sunday night. Every node reported `OutOfmemory` and `PIDPressure`, rendering the entire production cluster unresponsive for 4 hours.

### Root Cause
1. A billing report CronJob was scheduled to run every minute (`* * * * *`) with default `concurrencyPolicy: Allow`.
2. A database deadlock caused the billing report script to hang.
3. Every minute, the CronJob controller spawned a new Job pod without killing the old ones.
4. Over 6 hours, 360 concurrent heavy Java batch pods were created, consuming 100% of cluster RAM and kernel PIDs.

### Remediation
1. Configured `concurrencyPolicy: Forbid`.
2. Configured `activeDeadlineSeconds: 300` (5-minute hard timeout).
3. Configured `startingDeadlineSeconds: 60` to prevent delayed job pileups.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - History Limits CleanUp and Deadlines](./06-History-Limits-CleanUp-and-Deadlines.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
