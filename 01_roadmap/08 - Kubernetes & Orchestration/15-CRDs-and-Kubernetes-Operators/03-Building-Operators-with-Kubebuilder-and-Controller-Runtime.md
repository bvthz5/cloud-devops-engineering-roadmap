# 03 - Building Operators with Kubebuilder and Controller-Runtime

## 1. Go Operator Development with Kubebuilder

**Kubebuilder** is the official SDK for building production Kubernetes APIs and operators in Go:

```bash
# 1. Initialize new operator project
kubebuilder init --domain my-org.com --repo github.com/my-org/db-operator

# 2. Scaffold new API and Controller
kubebuilder create api --group apps --version v1 --kind Database

# 3. Generate CRD manifests from Go structs
make manifests

# 4. Run controller locally pointing to your test cluster
make run
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - The Kubernetes Operator Pattern and Control Loop](./02-The-Kubernetes-Operator-Pattern-and-Control-Loop.md) | [Index](../../../README.md) | [04 - Operator Reconciliation Loops and Event Handling →](./04-Operator-Reconciliation-Loops-and-Event-Handling.md) |
