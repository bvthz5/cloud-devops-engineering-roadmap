# 09 - Interview Q&A: AWS Container Services

### Q: What is IRSA in Amazon EKS and why is it preferred over attaching IAM policies to EC2 worker node roles?
**Answer:** IRSA (IAM Roles for Service Accounts) uses OIDC to associate IAM roles directly with Kubernetes pod service accounts, enforcing least-privilege security per microservice rather than sharing node-level permissions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
