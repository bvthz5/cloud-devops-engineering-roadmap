# 03 - S3 Lifecycle Rules

Automate transitions between storage classes to reduce costs:
```yaml
Standard ──(30 days)──> Standard-IA ──(90 days)──> Glacier Deep Archive ──(365 days)──> Expire
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - S3 Storage Classes Standard IA Glacier Deep Archive](./02-S3-Storage-Classes-Standard-IA-Glacier-Deep-Archive.md) | [Index](../../../README.md) | [04 - S3 Security Bucket Policies ACLs and Block Public Access →](./04-S3-Security-Bucket-Policies-ACLs-and-Block-Public-Access.md) |
