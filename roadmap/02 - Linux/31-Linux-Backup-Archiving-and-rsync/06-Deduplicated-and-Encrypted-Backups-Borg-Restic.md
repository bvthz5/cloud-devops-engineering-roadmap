# 06 — Deduplicated and Encrypted Backups: Restic and Borg

Traditional tar/rsync full backups waste terabytes storing duplicate data. Modern cloud-native backup utilities like **Restic** use content-addressable storage to eliminate duplicates and encrypt data client-side before sending it to AWS S3 or MinIO.

---

## 1. Restic Quick-Start with Cloud Storage

```bash
# 1. Initialize encrypted repository on AWS S3
export AWS_ACCESS_KEY_ID="my_access_key"
export AWS_SECRET_ACCESS_KEY="my_secret_key"
export RESTIC_PASSWORD="strong_master_encryption_password"
export RESTIC_REPOSITORY="s3:s3.amazonaws.com/company-backups/node01"

restic init

# 2. Take a snapshot backup of /data
restic backup /data

# 3. List historical snapshots
restic snapshots

# 4. Prune old backups with smart retention policy
restic forget --keep-daily 7 --keep-weekly 4 --keep-monthly 12 --prune
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Disaster Recovery RPO & RTO](./05-Disaster-Recovery-Strategies-RPO-and-RTO.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
