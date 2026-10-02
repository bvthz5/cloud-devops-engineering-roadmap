# MCQ - Google Cloud IAM & Service Accounts

> **Module**: Google Cloud IAM & Service Accounts

---

### Question 1
Which level of the GCP Resource Hierarchy is mandatory for containing all GCP resources?
- [ ] A) Folder
- [x] B) Project
- [ ] C) Organization
- [ ] D) Billing Account

*Explanation: A Project is the fundamental container for all GCP resources. Resources cannot exist outside of a Project.*

---

### Question 2
You need to grant a user permission to view all resources in a GCP project without giving them administrative modification access. Which role should you assign?
- [ ] A) `roles/owner`
- [ ] B) `roles/editor`
- [x] C) `roles/viewer`
- [ ] D) `roles/iam.securityAdmin`

*Explanation: The Primitive `roles/viewer` role grants read-only access to existing resources across the project.*

---

### Question 3
What is the primary security benefit of using Workload Identity Federation instead of Service Account Keys for GitHub Actions pipelines?
- [ ] A) Faster pipeline execution speeds
- [x] B) Eliminates the need to store long-lived static credentials in GitHub Secrets
- [ ] C) Automatically builds Docker images inside GCP
- [ ] D) Bypasses GCP IAM permission checks

*Explanation: Workload Identity Federation uses short-lived tokens, eliminating long-lived static JSON service account keys.*
