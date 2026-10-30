# 02 - S3 Storage Classes: Standard, Intelligent-Tiering, Standard-IA, Glacier & Deep Archive

> **Focus:** Economic optimization across the 6 S3 storage tiers, durability ratings, retrieval speeds, minimum storage retention durations, and per-GB pricing models.

---

## 🗄️ 1. Master S3 Storage Classes Spectrum

```text
Access Frequency: HIGH ◄────────────────────────────────────────────────► RARE / COLD
Storage Cost:     HIGH ◄────────────────────────────────────────────────► CHEAPEST ($0.00099/GB)
Retrieval Speed:  INSTANT ◄─────────────────────────────────────────────► HOURS

┌───────────────────┐  ┌───────────────────┐  ┌───────────────────┐  ┌───────────────────┐
│    S3 STANDARD    │  │S3 INTELLIGENT-TIER│  │   S3 GLACIER      │  │  GLACIER DEEP     │
│                   │  │                   │  │    FLEXIBLE       │  │     ARCHIVE       │
│ Active web assets,│  │ Auto-moves data   │  │ Cold compliance   │  │ Deep legal        │
│ daily big data.   │  │ based on access;  │  │ archives;         │  │ archives;         │
│ Instant access.   │  │ NO retrieval fees!│  │ 1 to 5 hrs access │  │ 12 to 48 hrs      │
└───────────────────┘  └───────────────────┘  └───────────────────┘  └───────────────────┘
```

### 📊 Comprehensive Storage Classes Comparison Matrix:

| Storage Class | Durability | Availability | Min Duration | Retrieval Latency | Best Enterprise Workload |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **S3 Standard** | **11 Nines** (`99.999999999%`) | `99.99%` | None | Milliseconds | Active web applications, live video streaming, big data analytics. |
| **S3 Intelligent-Tiering**| **11 Nines** | `99.9%` | None | Milliseconds | **Default for unpredictable data!** Moves between 5 access tiers automatically with zero retrieval fees. |
| **S3 Standard-IA** | **11 Nines** | `99.9%` | **30 Days** | Milliseconds | Long-term backups, disaster recovery data accessed once or twice a month. |
| **S3 One Zone-IA** | **11 Nines** | `99.5%` | **30 Days** | Milliseconds | Secondary non-critical backups stored in a single AZ (20% cheaper than Standard-IA). |
| **S3 Glacier Instant** | **11 Nines** | `99.9%` | **90 Days** | **Milliseconds** | Medical imaging, news archives accessed quarterly with instant retrieval. |
| **S3 Glacier Flexible** | **11 Nines** | `99.9%` | **90 Days** | **1 to 5 Hours** | Archival data requiring 1–5 hour bulk retrieval window. |
| **S3 Glacier Deep Archive**| **11 Nines**| `99.9%` | **180 Days**| **12 to 48 Hours**| **Cheapest cloud storage ($0.00099/GB/mo)**. 7-year regulatory compliance archives. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - S3 Architecture Buckets Objects and Namespaces](./01-S3-Architecture-Buckets-Objects-and-Namespaces.md) | [Index](../../../README.md) | [03 - S3 Lifecycle Rules Expiration and Transition Policies →](./03-S3-Lifecycle-Rules-Expiration-and-Transition-Policies.md) |
