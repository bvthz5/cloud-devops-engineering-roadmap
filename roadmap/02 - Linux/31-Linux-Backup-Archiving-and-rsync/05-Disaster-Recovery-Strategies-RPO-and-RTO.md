# 05 — Disaster Recovery Strategies: RPO and RTO

Every business disaster recovery (DR) architecture is defined by two metrics: **RPO** and **RTO**.

---

## 1. RPO vs RTO

```text
Last Backup             Disaster Strikes            Service Restored
    |                          |                           |
    +-------- RPO -------------+------------ RTO ----------+
           Data Lost                  Downtime Duration
```

- **RPO (Recovery Point Objective):** The maximum acceptable amount of data loss measured in time (e.g., "We can tolerate losing at most 15 minutes of transactions"). Dictates backup frequency (continuous replication vs hourly vs daily).
- **RTO (Recovery Time Objective):** The maximum acceptable duration of downtime before service is restored (e.g., "The site must be back online within 1 hour"). Dictates restoration automation (automated failover vs manual restore).

---

## 2. The 3-2-1-1-0 Backup Rule

- **3** Copies of data (1 primary production + 2 backups).
- **2** Different media types (e.g. Local NVMe SSD + Cloud Object Storage).
- **1** Copy stored offsite in a geographically separated region.
- **1** Copy stored **Immutable / Air-Gapped** (WORM - Write Once, Read Many; protection against ransomware).
- **0** Errors verified via automated restoration drills.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Snapshot-Based Backups](./04-Snapshot-Based-Backups-LVM-and-Btrfs.md) | [README](./README.md) | [06 - Deduplicated Backups](./06-Deduplicated-and-Encrypted-Backups-Borg-Restic.md) |
