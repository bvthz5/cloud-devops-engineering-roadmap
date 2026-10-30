# Interview Q&A - Kubecost & OpenCost: K8s Cost Optimization

> **Module**: Kubecost & OpenCost: K8s Cost Optimization

---

### Q1: What is the FinOps Lifecycle and what are its three phases?
**Answer**:
The FinOps Lifecycle is an iterative management framework consisting of:
1. **Inform**: Giving teams visibility into costs, unit economics, allocation tagging, and benchmarking.
2. **Optimize**: Reducing waste, rightsizing workloads, and optimizing rates (Savings Plans, Reserved Instances, Spot VMs).
3. **Operate**: Aligning organizational goals, defining KPIs, and automating continuous governance and budget guardrails.

---

### Q2: What is the difference between Chargeback and Showback?
**Answer**:
- **Showback**: Provides visibility into cloud usage costs by department/team without actually transferring funds or deducting budgets. It builds cost awareness.
- **Chargeback**: Directly cross-charges cloud infrastructure costs to the respective department or product budget, establishing financial accountability.

---

### Q3: How does Kubecost calculate resource costs on shared Kubernetes clusters?
**Answer**:
Kubecost integrates with cloud provider billing APIs (AWS, Azure, GCP) to get exact node and disk pricing. It then tracks individual Pod container CPU/Memory **requests and actual usage** over time, calculating exact monetary cost per Pod, Namespace, Service, or Label.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
