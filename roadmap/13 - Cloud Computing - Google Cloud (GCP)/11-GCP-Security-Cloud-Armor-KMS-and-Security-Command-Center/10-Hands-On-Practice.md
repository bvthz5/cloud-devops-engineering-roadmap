# Hands-On Practice - GCP Security: Cloud Armor, KMS & Security Command Center

> **Module**: GCP Security: Cloud Armor, KMS & Security Command Center

---

## 🛠 Lab: Secure Infrastructure Automation with Secret Manager & Terraform

### Objective
In this lab, you will configure a GCS remote backend for Terraform, create a Secret Manager secret using the `gcloud` CLI, and securely consume secrets inside Terraform scripts.

---

## 📋 Task 1: Create Remote State GCS Bucket
```bash
# Create dedicated bucket for Terraform state
gcloud storage buckets create gs://tf-state-$DEVSHELL_PROJECT_ID \
    --location=us-central1 \
    --uniform-bucket-level-access
```

---

## 📋 Task 2: Create Secret in Secret Manager
```bash
# Create secret and add secret version
gcloud secrets create api-token --replication-policy="automatic"
echo -n "ghp_ProdApiSecretToken998877" | gcloud secrets versions add api-token --data-file=-
```

---

## 📋 Task 3: Access Secret via Terraform
```hcl
# Access existing Secret Manager secret version
data "google_secret_manager_secret_version" "api_token" {
  secret = "api-token"
}

output "secret_version_id" {
  value     = data.google_secret_manager_secret_version.api_token.name
  sensitive = true
}
```
