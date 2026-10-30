# Google Cloud Architecture Framework: 5 Pillars

> **Module**: GCP Cost Management, FinOps & Architecture Framework  
> **Level**: Production & Deep-Dive  

---

## 📌 Executive Summary
Understanding **Google Cloud Architecture Framework: 5 Pillars** is crucial for designing, building, and operating production systems on Google Cloud Platform (GCP). This guide covers underlying cloud mechanics, enterprise deployment configurations, security controls, and operational workflows.

---

## 🏗 Architecture & Core Mechanics

```
+-------------------------------------------------------------------------------+
|                        GCP Global Network & Resource Scope                   |
+-------------------------------------------------------------------------------+
|  Organization  -->  Folders  -->  Projects  -->  Resources (VPC, GCE, GKE)   |
|  IAM Policies  -->  Org Policies  -->  Billing Accounts  --> Audit Logs       |
+-------------------------------------------------------------------------------+
```

### Key Concepts
1. **Security & Perimeter Defense**: VPC Service Controls perimeters, Cloud Armor WAF rules, and KMS encryption management.
2. **Infrastructure as Code**: Managing GCP declarative state using Terraform Google Provider, Cloud Build pipelines, and Kubernetes Config Connector.
3. **Hybrid Cloud Governance**: Multi-cluster fleet management via Anthos (GKE Enterprise) and Anthos Config Management (ACM) GitOps.
4. **FinOps & Cost Governance**: Exporting granular billing data to BigQuery, utilizing Committed Use Discounts (CUDs), and enforcing budget alert notifications.

---

## 🛠 Command Line & Automation Examples

### `gcloud` CLI Management
```bash
# Export secrets securely using Secret Manager
gcloud secrets create db-password --replication-policy="automatic"
echo -n "SuperSecretPass123" | gcloud secrets versions add db-password --data-file=-

# Configure Cloud KMS Key Ring and CryptoKey
gcloud kms keyrings create prod-keyring --location=us-central1
gcloud kms keys create app-key --location=us-central1 --keyring=prod-keyring --purpose=encryption
```

### Terraform Infrastructure as Code (GCS Backend & CMEK)
```hcl
terraform {
  backend "gcs" {
    bucket = "terraform-state-prod-bucket"
    prefix = "infrastructure/state"
  }
}

resource "google_storage_bucket" "secure_bucket" {
  name                        = "enterprise-data-secure-bucket"
  location                    = "US"
  uniform_bucket_level_access = true

  encryption {
    default_kms_key_name = google_kms_crypto_key.app_key.id
  }
}
```

---

## 💡 Production Best Practices
- **VPC Service Controls**: Create security perimeters around sensitive data APIs (BigQuery, GCS) to prevent unauthorized data exfiltration.
- **GitOps for Multi-Cluster**: Use Anthos Config Management (ACM) to sync Kubernetes manifests across hybrid/multi-cloud GKE clusters from a single Git source of truth.
- **FinOps Automation**: Export GCP billing export to BigQuery daily and schedule Looker Studio dashboards to track spend anomalies.
- **CMEK Lifecycle**: Rotate Customer-Managed Encryption Keys in Cloud KMS on an automated annual cycle.

---

## 🔗 Related Resources
- [Google Cloud Security Documentation](https://cloud.google.com/security/docs)
- [GCP FinOps & Cost Management](https://cloud.google.com/cost-management)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Active Assist and Recommender Cost Optimization](./04-Active-Assist-and-Recommender-Cost-Optimization.md) | [Index](../../../README.md) | [06 - GCP Landing Zones Fabric Fast and Enterprise Foundation →](./06-GCP-Landing-Zones-Fabric-Fast-and-Enterprise-Foundation.md) |
