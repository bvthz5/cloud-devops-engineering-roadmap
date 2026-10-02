# Hands-On Practice - Multi-Cloud Identity, Access & Unified Governance

> **Module**: Multi-Cloud Identity, Access & Unified Governance

---

## 🛠 Lab: Multi-Cloud Workload Identity Federation (AWS & GCP)

### Objective
In this lab, you will configure an AWS IAM OIDC Provider and Role to allow a GCP Service Account (or GitHub Actions) to assume AWS IAM permissions dynamically without static access keys.

---

## 📋 Task 1: Create AWS IAM OIDC Provider
```bash
# Register GCP OIDC provider in AWS IAM
aws iam create-open-id-connect-provider     --url "https://accounts.google.com"     --client-id-list "https://iam.googleapis.com"     --thumbprint-list "6938fd4d98bab03faadb97b34396831e3780aea1"
```

---

## 📋 Task 2: Define Trust Policy JSON
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::ACCOUNT_ID:oidc-provider/accounts.google.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "accounts.google.com:sub": "GCP_SERVICE_ACCOUNT_UNIQUE_ID"
        }
      }
    }
  ]
}
```

---

## 📋 Task 3: Create Role and Attach Read-Only Policy
```bash
# Create IAM role with trust policy
aws iam create-role     --role-name MultiCloudGcpAccessRole     --assume-role-policy-document file://trust-policy.json

# Attach S3 Read Only Access
aws iam attach-role-policy     --role-name MultiCloudGcpAccessRole     --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```
