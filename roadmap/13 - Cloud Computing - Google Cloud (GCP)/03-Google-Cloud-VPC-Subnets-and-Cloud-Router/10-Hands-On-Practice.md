# Hands-On Practice - Google Cloud VPC, Subnets & Cloud Router

> **Module**: Google Cloud VPC, Subnets & Cloud Router

---

## 🛠 Lab: Building a Secure, Automated GCP Environment with gcloud & Terraform

### Objective
In this lab, you will configure a custom GCP network, set up dedicated Service Accounts with minimal IAM permissions, and verify resource deployments using the `gcloud` CLI.

---

## 📋 Task 1: Environment Context Setup
```bash
# Set default project and region context
gcloud config set project YOUR_PROJECT_ID
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
```

---

## 📋 Task 2: Provision Custom VPC & Firewall Rules
```bash
# Create custom VPC
gcloud compute networks create prod-vpc --subnet-mode=custom

# Create subnet with Private Google Access enabled
gcloud compute networks subnets create prod-app-subnet \
    --network=prod-vpc \
    --region=us-central1 \
    --range=10.10.1.0/24 \
    --enable-private-ip-google-access

# Create internal SSH firewall rule via IAP
gcloud compute firewall-rules create allow-iap-ssh \
    --network=prod-vpc \
    --allow=tcp:22 \
    --source-ranges=35.235.240.0/20
```

---

## 📋 Task 3: Verify & Clean Up
```bash
# Verify subnet details
gcloud compute networks subnets describe prod-app-subnet --region=us-central1

# Delete resources after verification
gcloud compute firewall-rules delete allow-iap-ssh --quiet
gcloud compute networks subnets delete prod-app-subnet --region=us-central1 --quiet
gcloud compute networks delete prod-vpc --quiet
```
