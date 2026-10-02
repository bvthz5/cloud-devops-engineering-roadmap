# 02 - S3 Storage Classes

| Storage Class | Durability | Availability | Min Storage Duration |
|---|---|---|---|
| **S3 Standard** | 99.999999999% | 99.99% | None |
| **S3 Intelligent-Tiering** | 99.999999999% | 99.9% | None (Auto-tiers based on access) |
| **S3 Standard-IA** | 99.999999999% | 99.9% | 30 days |
| **S3 Glacier Flexible** | 99.999999999% | 99.9% | 90 days (Retrieval: mins to hours) |
| **S3 Glacier Deep Archive**| 99.999999999% | 99.9% | 180 days (Retrieval: 12 hours) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - S3 Architecture](./01-S3-Architecture-Buckets-Objects-and-Namespaces.md) | [README](./README.md) | [03 - Lifecycle Rules](./03-S3-Lifecycle-Rules-Expiration-and-Transition-Policies.md) |
