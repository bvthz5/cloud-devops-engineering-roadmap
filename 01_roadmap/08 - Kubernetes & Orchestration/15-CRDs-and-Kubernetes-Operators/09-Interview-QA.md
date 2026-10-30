# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between a Controller and an Operator?
**Answer:**
All Operators are Controllers, but not all Controllers are Operators.
- A **Controller** is any core control loop that reconciles desired state with actual state (e.g. `DeploymentController` managing ReplicaSets).
- An **Operator** is a specialized controller that manages **domain-specific, stateful applications** (e.g. Postgres, Kafka, Elasticsearch) encoding operational human knowledge (automated backups, failovers, schema upgrades, clustering) using Custom Resource Definitions (CRDs).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
