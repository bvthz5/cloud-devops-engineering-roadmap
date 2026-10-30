# 02 - The Kubernetes Operator Pattern and Control Loop

## 1. What Is an Operator?

A CRD alone is merely static data in `etcd`.
An **Operator** is a custom controller that watches that CRD and takes real actions:
`Operator = Custom Resource Definition (CRD) + Custom Controller`

```text
[ User submits: Database CRD (replicas: 3, engine: postgres) ]
                         │
                         ▼
[ Custom Operator Controller watches CRD via API Server ]
                         │
                         ▼
              Reconciliation Loop:
   ├── Checks actual state: 0 pods running
   ├── Provisions StatefulSet with 3 pods
   ├── Configures primary-replica streaming replication
   ├── Provisions cloud backup snapshot CronJob
   └── Updates CRD Status: "Ready"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - CustomResourceDefinitions CRDs and OpenAPI v3 Validation](./01-CustomResourceDefinitions-CRDs-and-OpenAPI-v3-Validation.md) | [Index](../../../README.md) | [03 - Building Operators with Kubebuilder and Controller Runtime →](./03-Building-Operators-with-Kubebuilder-and-Controller-Runtime.md) |
