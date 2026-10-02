# 06 - Operator Lifecycle Manager (OLM) and OperatorHub

## 1. What Is OLM?

The **Operator Lifecycle Manager (OLM)** manages the installation, automatic upgrades, and dependency resolution of operators across a cluster:
- **OperatorHub.io:** Public catalog of enterprise-ready operators (Prometheus, Vault, Kafka Strimzi, PostgreSQL CloudNative-PG).
- **Subscription:** Declares desired operator channel (e.g. `stable`, `fast`) and manages automated background updates.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Finalizers and Safe Resource Teardown](./05-Finalizers-and-Safe-Resource-Teardown.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
