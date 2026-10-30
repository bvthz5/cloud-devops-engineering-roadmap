# 01 - Amazon EC2 Instance Types, Families, Generations & Graviton Processors

> **Focus:** Virtual server compute classifications, instance naming decoder (`c6g.2xlarge`), instance families (General Purpose, Compute, Memory, Storage, GPU), and AWS Graviton ARM price-performance.

---

## 🏷️ 1. EC2 Instance Naming Convention Decoded

```text
         c 6 g . 2 x l a r g e
         │ │ │   │
         │ │ │   └── Sizing Multiplier: 2xlarge = 8 vCPUs, 16 GB RAM
         │ │ └────── Processor / Feature: "g" = AWS Graviton (ARM64 Architecture)
         │ └──────── Hardware Generation: 6th Generation (Newer = Faster & Cheaper)
         └────────── Family Category: "c" = Compute Optimized
```

---

## 📊 2. Master EC2 Instance Families Reference

| Family | Series | CPU : RAM Ratio | Best Enterprise Workloads | Analogy |
| :--- | :---: | :---: | :--- | :--- |
| **General Purpose (Burstable)** | `t3`, `t4g` | Dynamic | Dev/test servers, small blogs, microservices, jump boxes. Uses CPU credits. | Scooter with a quick turbo boost button. |
| **General Purpose (Balanced)** | `m5`, `m6i`, `m7g` | `1 : 4` | Production web backends, application servers, enterprise microservices. | Reliable family sedan for daily driving. |
| **Compute Optimized** | `c5`, `c6i`, `c7g` | `1 : 2` | Batch processing, video encoding, gaming servers, high-performance web servers. | Formula 1 sports car built for pure engine speed. |
| **Memory Optimized** | `r5`, `r6i`, `r7g` | `1 : 8` | In-memory caches (Redis, Memcached), high-load relational databases, SAP HANA. | Giant cruise ship built to carry massive cargo. |
| **Storage / IOPS Optimized**| `i3`, `i4i` | NVMe local | High-transaction NoSQL (Cassandra, MongoDB), Elasticsearch, big data warehousing. | Heavy-duty commercial freight train. |
| **GPU / Accelerated** | `g4`, `g5`, `p4`, `p5`| NVIDIA GPUs | Deep learning / LLM training, autonomous vehicle simulation, 3D CAD rendering. | Professional Hollywood visual effects studio. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-AWS-VPC-Subnets-Routing-Gateways)](../04-AWS-VPC-Subnets-Routing-Gateways/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - EC2 Pricing Models OnDemand Spot Reserved SavingsPlans →](./02-EC2-Pricing-Models-OnDemand-Spot-Reserved-SavingsPlans.md) |
