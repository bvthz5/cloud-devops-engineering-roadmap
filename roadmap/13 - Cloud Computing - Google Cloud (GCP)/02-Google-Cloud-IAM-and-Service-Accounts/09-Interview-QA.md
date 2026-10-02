# Interview Q&A - Google Cloud IAM & Service Accounts

> **Module**: Google Cloud IAM & Service Accounts

---

### Q1: What is the GCP Resource Hierarchy and why is it important for cloud governance?
**Answer**:
The GCP Resource Hierarchy consists of **Organization -> Folders -> Projects -> Resources**. It provides a logical structure for managing access control (IAM), organization policies, and billing. Policies set at higher levels (e.g., Organization or Folder) are inherited down the hierarchy tree, allowing central security teams to enforce compliance across all projects effortlessly.

---

### Q2: What is the difference between Service Account Impersonation and Service Account Keys?
**Answer**:
- **Service Account Keys**: Static JSON credentials stored locally or in code repositories. They pose significant security risks if leaked and require manual key rotation.
- **Service Account Impersonation**: Uses short-lived OAuth 2.0 access tokens generated dynamically by IAM (`roles/iam.serviceAccountTokenCreator`). It eliminates static credentials, improving security posture and complying with zero-trust principles.

---

### Q3: Explain the difference between Shared VPC and VPC Peering in GCP.
**Answer**:
- **Shared VPC**: Connects projects within the *same* GCP Organization. A central host project manages the network infrastructure (VPCs, subnets, firewalls, routes), while service projects instantiate workloads (VMs, GKE) inside those subnets.
- **VPC Peering**: Connects two independent VPC networks (which can belong to different organizations) using internal IP addressing. Traffic does not traverse the public internet, but administrative control remains separate.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
