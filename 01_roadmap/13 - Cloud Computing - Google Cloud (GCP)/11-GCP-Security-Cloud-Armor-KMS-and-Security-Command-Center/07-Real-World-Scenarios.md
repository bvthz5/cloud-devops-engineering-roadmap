# Real-World Scenarios - GCP Security: Cloud Armor, KMS & Security Command Center

> **Module**: GCP Security: Cloud Armor, KMS & Security Command Center

---

## 🏢 Scenario 1: Enterprise Data Exfiltration Prevention via VPC Service Controls

### Background
A regulated healthcare firm storing patient records in BigQuery and GCS needs to guarantee that data cannot be copied to external unauthorized GCP storage buckets or downloaded over public networks, even by compromised admin credentials.

### Solution Architecture
1. **Security Perimeter**: Define a VPC Service Control perimeter encompassing the data project containing BigQuery and GCS resources.
2. **Access Levels**: Restrict API communication so access to the perimeter is allowed only from corporate IP addresses and authorized Shared VPC subnets.
3. **Inbound/Outbound Rules**: Enforce explicit ingress and egress policies prohibiting exfiltration to external GCP projects outside the perimeter boundary.

---

## 🏢 Scenario 2: Multi-Cloud Hybrid GitOps Governance with Anthos & ACM

### Background
A enterprise organization operates Kubernetes clusters across GCP (GKE), AWS (EKS), and on-premises data centers. They need consistent RBAC roles, security policies, and application configurations synced across all clusters automatically.

### Solution Architecture
1. **Fleet Registration**: Register all GKE and external Kubernetes clusters to an Anthos Fleet in the GCP console.
2. **Anthos Config Management**: Connect ACM to a central Git repository containing Kubernetes policy definitions (Gatekeeper OPA constraint templates).
3. **GitOps Synchronization**: ACM continuously monitors the Git repository and automatically applies policy updates across all registered hybrid clusters within seconds.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Secret Manager Centralized Secret Management and Rotation](./06-Secret-Manager-Centralized-Secret-Management-and-Rotation.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
