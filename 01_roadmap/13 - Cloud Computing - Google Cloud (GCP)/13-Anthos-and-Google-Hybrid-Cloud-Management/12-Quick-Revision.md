# Quick Revision Notes - Anthos & Google Hybrid Cloud Management

> **Module**: Anthos & Google Hybrid Cloud Management

---

## ⚡ Key Cheat Sheet

| Topic | Key Function | Core Highlight |
|---|---|---|
| **Cloud KMS** | Encryption key management | Supports CMEK, automated key rotation |
| **VPC Service Controls** | Security perimeters | Prevents data exfiltration across GCP APIs |
| **Config Connector** | Kubernetes-native GCP IaC | Manages GCP resources via CRDs inside GKE |
| **Anthos Fleet** | Multi-cluster management | Centralized control of GKE and hybrid clusters |
| **BigQuery Billing Export** | Granular FinOps analytics | Query detailed cloud costs using SQL |

---

## 📝 Top 5 Rules to Remember
1. **Always use GCS Remote Backend with Object Versioning** for team-based Terraform state management.
2. **Store sensitive credentials in Secret Manager**, never hardcoded in Terraform code or container images.
3. **Use VPC Service Controls for strict compliance data perimeters** (healthcare, financial data).
4. **Enable BigQuery Billing Export immediately** to enable granular cost visualization and FinOps analysis.
5. **Use Anthos Fleet and ACM for GitOps** across hybrid Kubernetes deployments.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (14-GCP-Cost-Management-FinOps-and-Architecture-Framework) →](../14-GCP-Cost-Management-FinOps-and-Architecture-Framework/01-GCP-Billing-Architecture-Accounts-Billing-Projects-Exports.md) |
