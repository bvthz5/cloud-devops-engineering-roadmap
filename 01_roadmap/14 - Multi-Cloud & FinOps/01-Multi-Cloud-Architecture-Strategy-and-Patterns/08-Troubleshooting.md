# Troubleshooting Guide - Multi-Cloud Architecture Strategy & Patterns

> **Module**: Multi-Cloud Architecture Strategy & Patterns

---

## 🔍 Common Issue 1: Untagged Cloud Resources & Unattributed Spend

### Symptom
Monthly billing reports show a large percentage of spend labeled as `Unallocated` or `Other`, preventing accurate department chargebacks.

### Root Cause & Resolution
1. **Legacy Resources**: Pre-existing infrastructure created manually without tagging standards.
2. **Automated Tag Remediation**: Deploy Tagging Enforcer scripts (AWS Config Rules, Azure Policy, GCP Custodian) to auto-tag resources based on creator IAM identities.
3. **Pipeline Enforcement**: Fail CI/CD builds if Terraform modules lack mandatory tag blocks.

---

## 🔍 Common Issue 2: Unexpected Cloud Egress Charges in Multi-Cloud Setup

### Symptom
Data transfer costs spike unexpectedly between AWS S3 and GCP BigQuery pipelines.

### Root Cause & Resolution
1. **Public Internet Routing**: Data replication is traversing public internet endpoints, incurring standard cloud egress fees (\$0.09/GB).
2. **Private Interconnect**: Configure Megaport / Equinix Cloud Exchange or dedicated DirectConnect / Partner Interconnect routes to lower data transfer costs by up to 60%.
3. **Data Compression & Caching**: Compress data batches (e.g., Parquet/Avro) before cross-cloud transmission.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
