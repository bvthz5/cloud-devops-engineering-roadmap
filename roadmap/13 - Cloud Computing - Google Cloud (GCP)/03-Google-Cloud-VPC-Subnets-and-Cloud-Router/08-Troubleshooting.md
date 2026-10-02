# Troubleshooting Guide - Google Cloud VPC, Subnets & Cloud Router

> **Module**: Google Cloud VPC, Subnets & Cloud Router

---

## 🔍 Common Issue 1: IAM Permission Denied (`403 Forbidden`)

### Symptom
Applications or administrators receive `googleapi: Error 403: Insufficient Permission` when interacting with GCP APIs or resources.

### Root Cause & Resolution
1. **Missing Role Binding**: Check if the user or Service Account has the specific API permission required. Run:
   ```bash
   gcloud asset search-all-iam-policies --query="policy:sa-app@PROJECT_ID.iam.gserviceaccount.com"
   ```
2. **Organization Policy Constraint**: Verify if an Org Policy (e.g., Domain Restricted Sharing) blocks the assignment.
3. **Scope Issues**: If running on GCE or GKE, verify that the VM instance API access scope includes `cloud-platform`.

---

## 🔍 Common Issue 2: Network Connectivity Failure & Private Access Timeout

### Symptom
VMs without external IP addresses fail to download package updates or access GCP services (e.g., Cloud Storage, BigQuery).

### Root Cause & Resolution
1. **Private Google Access Disabled**: Ensure Private Google Access is enabled on the subnet.
   ```bash
   gcloud compute networks subnets update SUBNET_NAME \
       --region=us-central1 \
       --enable-private-ip-google-access
   ```
2. **Missing Cloud NAT**: For external internet outbound traffic, ensure Cloud Router and Cloud NAT are configured for the VPC region.
3. **Firewall Egress Rule**: Check if custom egress firewall rules are blocking outbound traffic on port 443.
