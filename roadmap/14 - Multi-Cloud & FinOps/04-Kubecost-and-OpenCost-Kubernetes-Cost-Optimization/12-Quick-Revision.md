# Quick Revision Notes - Kubecost & OpenCost: K8s Cost Optimization

> **Module**: Kubecost & OpenCost: K8s Cost Optimization

---

## ⚡ Key Cheat Sheet

| FinOps Concept | Core Function | Key Tool / Standard |
|---|---|---|
| **Inform Phase** | Visibility, tagging, dashboards | AWS CUR, Azure Cost, GCP BigQuery, FOCUS |
| **Optimize Phase** | Rightsizing, Spot instances, Savings Plans | Kubecost, AWS Compute Optimizer, Cloud Advisor |
| **Operate Phase** | Continuous governance, guardrails | Infracost, OPA, Sentinel, Custodian |
| **Unit Economics** | Cost per business metric | Spend / Active Users (or Transactions) |
| **Kubernetes FinOps** | Pod-level cost allocation | Kubecost / OpenCost |

---

## 📝 Top 5 Rules to Remember
1. **Tagging is non-negotiable**: Untagged spend cannot be attributed accurately.
2. **FinOps is a cultural practice**, not just a cost-cutting exercise.
3. **Shift-Left FinOps with Infracost** to catch financial bugs in pull requests before deploying.
4. **Use Spot/Preemptible instances for fault-tolerant workloads** (batch jobs, stateless microservices).
5. **Standardize multi-cloud billing schemas using FOCUS** for unified enterprise reporting.
