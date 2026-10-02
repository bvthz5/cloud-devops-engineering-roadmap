# Interview Q&A - GCP Cost Management, FinOps & Architecture Framework

> **Module**: GCP Cost Management, FinOps & Architecture Framework

---

### Q1: What is the difference between CMEK and CSEK in GCP?
**Answer**:
- **CMEK (Customer-Managed Encryption Keys)**: You create, manage, and rotate encryption keys stored in Google Cloud KMS, but Google performs the cryptographic operations on your behalf.
- **CSEK (Customer-Supplied Encryption Keys)**: You generate and maintain cryptographic keys completely outside of GCP on-premises. You supply the raw key with each API request, and Google uses it in memory to encrypt/decrypt data, never storing the key.

---

### Q2: How does Config Connector differ from Terraform on GCP?
**Answer**:
- **Terraform**: External IaC CLI tool that manages infrastructure via GCP REST APIs during deployment pipeline execution (`terraform apply`). State is stored in a backend file (e.g., GCS bucket).
- **Config Connector**: Kubernetes add-on that allows you to manage GCP resources as native Kubernetes Custom Resources (CRDs) directly inside a GKE cluster. Kubernetes continuously reconciles desired infrastructure state.

---

### Q3: What are Committed Use Discounts (CUDs) in GCP FinOps?
**Answer**:
Committed Use Discounts (CUDs) offer significant cost reductions (up to 57% for 1-year or 70% for 3-year commitments) in exchange for committing to a minimum level of resource usage (e.g., compute vCPUs, RAM, Cloud SQL, BigQuery slots). CUDs apply automatically to qualifying resources without needing to recreate or modify existing infrastructure.
