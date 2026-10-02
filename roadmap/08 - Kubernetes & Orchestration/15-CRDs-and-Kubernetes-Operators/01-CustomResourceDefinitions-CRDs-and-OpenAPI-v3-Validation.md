# 01 - CustomResourceDefinitions (CRDs) and OpenAPI v3 Validation

## 1. Extending Kubernetes

A **CustomResourceDefinition (CRD)** teaches `kube-apiserver` about a completely new resource type (e.g. `PostgreSQLCluster`, `KafkaTopic`, `VirtualService`) without modifying core Kubernetes.

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.databases.example.com
spec:
  group: databases.example.com
  names:
    kind: Database
    listKind: DatabaseList
    plural: databases
    singular: database
    shortNames:
    - db
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            required: ["engine", "replicas"]
            properties:
              engine:
                type: string
                enum: ["postgres", "mysql"]
              replicas:
                type: integer
                minimum: 1
                maximum: 5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - The Operator Pattern](./02-The-Kubernetes-Operator-Pattern-and-Control-Loop.md) |
