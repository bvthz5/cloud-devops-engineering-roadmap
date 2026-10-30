# Quick Revision Notes - Google Cloud Storage (GCS) & Cloud Filestore

> **Module**: Google Cloud Storage (GCS) & Cloud Filestore

---

## ⚡ Key Cheat Sheet

| Concept | Key Summary | Exam / Interview Note |
|---|---|---|
| **Resource Hierarchy** | Org -> Folders -> Projects -> Resources | IAM policies inherit top-down |
| **Service Accounts** | Identities used by applications/workloads | Avoid static keys; use Impersonation or Workload Identity |
| **VPC Scope** | Global resource in GCP | Subnets are regional resources |
| **Private Access** | Enable on subnet for VMs without External IPs | Required to access Google APIs internally |
| **Shared VPC** | Host project manages network, Service projects host VMs | Ideal for centralized enterprise networking |

---

## 📝 Top 5 Rules to Remember
1. **Never use Primitive Roles (`Owner`/`Editor`) in production**. Always use Predefined or Custom roles.
2. **Every GCP Resource belongs to exactly one Project**.
3. **Subnets in GCP are Regional**, while the VPC network itself is Global.
4. **Service Account Keys should be audited regularly** or replaced with short-lived Workload Identity credentials.
5. **Organization Policies override IAM permissions** if an org policy constraint is violated.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-Cloud-SQL-Cloud-Spanner-and-Firestore) →](../06-Cloud-SQL-Cloud-Spanner-and-Firestore/01-Cloud-SQL-Managed-Relational-Databases-MySQL-PostgreSQL-SQL-Server.md) |
