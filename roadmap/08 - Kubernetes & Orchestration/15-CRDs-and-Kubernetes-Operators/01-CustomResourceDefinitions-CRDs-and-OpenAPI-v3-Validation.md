# 01 - CustomResourceDefinitions (CRDs) and OpenAPI v3 Validation

## 1. What Is a CustomResourceDefinition (CRD)?

A **CustomResourceDefinition (CRD)** is a declarative Kubernetes API extension mechanism that allows cluster engineers to define completely new, domain-specific object types in the cluster without writing a single line of core Kubernetes code or recompiling the `kube-apiserver`.

Kubernetes is architected as an extensible platform. Out of the box, Kubernetes understands core resources like `Pods`, `Services`, and `Deployments`. However, modern cloud-native systems require abstractions for application-layer domains, such as `PostgreSQLCluster`, `KafkaTopic`, `VaultSecret`, `CertManagerCertificate`, or `PrometheusRule`.

When you apply a CRD manifest:
1. The `apiextensions-apiserver` component inside the API server registers the new API group and version.
2. It dynamically exposes a brand-new REST endpoint (for example, `/apis/database.example.com/v1/namespaces/default/postgresclusters`).
3. You can immediately interact with the new resource using standard `kubectl` commands (`kubectl get postgresclusters`, `kubectl apply`, `kubectl describe`).
4. Custom resources are persisted directly into `etcd` alongside native Kubernetes objects.

```text
+---------------------------------------------------------------------------------------------------+
|                        DYNAMIC API EXTENSION VIA CustomResourceDefinition                         |
+---------------------------------------------------------------------------------------------------+

 1. Administrator submits CRD Manifest:
 ┌────────────────────────────────────────────────────────┐
 │ kind: CustomResourceDefinition                         │
 │ group: database.example.com                            │
 │ kind: PostgresCluster                                  │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 2. kube-apiserver registers new REST endpoint in-flight:
 ┌────────────────────────────────────────────────────────┐
 │ HTTPS /apis/database.example.com/v1/postgresclusters   │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 3. Developer can now create and manage Custom Resources natively!
 ┌────────────────────────────────────────────────────────┐
 │ apiVersion: database.example.com/v1                    │
 │ kind: PostgresCluster                                  │
 │ metadata: name: production-db                          │
 │ spec: storage: 500Gi, engine: "15.4"                   │
 └────────────────────────────────────────────────────────┘
```

---

## 2. Structural Schema and OpenAPI v3 Validation

A CRD must define a **Structural OpenAPI v3 Schema**. The schema acts as a strict firewall that prevents malformed, invalid, or dangerous data from ever being written to `etcd`. If a developer submits a manifest containing missing required fields, negative integers, or unrecognized keys, the API server rejects the request at the admission gate with an `HTTP 422 Unprocessable Entity`.

### Core Validation Features Supported:
- **Type Safety:** Enforces strict scalar types (`string`, `integer`, `boolean`, `array`, `object`).
- **Enums & Patterns:** Restricts fields to explicit allowlists (`enum: ["postgres", "mysql"]`) or regex patterns (`pattern: "^[a-z0-9-]+$"`).
- **Numeric Boundaries:** Constrains ranges (`minimum: 1`, `maximum: 10`).
- **Required Fields:** Mandates essential attributes under `required: ["engine", "replicas"]`.

```text
+---------------------------------------------------------------------------------------------------+
|                         OPENAPI v3 ADMISSION VALIDATION GATEWAY                                   |
+---------------------------------------------------------------------------------------------------+

 Developer submits CR:
 ┌────────────────────────────────────┐
 │ kind: PostgresCluster              │
 │ spec:                              │
 │   replicas: -5   ◄─────────────────┼─── OpenAPI Validation: minimum: 1 violated!
 │   engine: "oracle" ◄───────────────┼─── OpenAPI Validation: enum [postgres, mysql] violated!
 └────────────────────────────────────┘
                  │
                  ▼
 kube-apiserver rejects submission with HTTP 422:
 "PostgresCluster.spec.replicas: Invalid value: -5: must be greater than or equal to 1"
 (Object rejected in memory; etcd database is never touched!)
```

---

## 3. Advanced CRD Subresources: `/status` and `/scale`

A production CRD should always declare the `/status` and `/scale` subresources under `spec.versions[*].subresources`:

### 1. The `/status` Subresource
- Decouples the user-defined `spec` from the controller-generated `status`.
- Regular developers are granted RBAC permissions to update `spec`, while the background operator Controller is granted permissions on the `status` subresource.
- Prevents race conditions and prevents status updates from incrementing the object's `metadata.generation` number.

### 2. The `/scale` Subresource
- Exposes standard replica scaling hooks (`specReplicasPath`, `statusReplicasPath`).
- Enables the custom resource to integrate seamlessly with the **Horizontal Pod Autoscaler (HPA)** and `kubectl scale`:
  ```bash
  kubectl scale postgrescluster production-db --replicas=5
  ```

---

## 4. Production Manifest: PostgresCluster CustomResourceDefinition

The following complete manifest defines an enterprise-grade CRD with OpenAPI v3 validation, custom printer columns, and scaling subresources:

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: postgresclusters.database.example.com
spec:
  group: database.example.com
  names:
    kind: PostgresCluster
    listKind: PostgresClusterList
    plural: postgresclusters
    singular: postgrescluster
    shortNames:
      - pg
      - pgcluster
  scope: Namespaced
  versions:
    - name: v1
      served: true
      storage: true
      subresources:
        status: {}
        scale:
          specReplicasPath: .spec.replicas
          statusReplicasPath: .status.readyReplicas
          labelSelectorPath: .status.labelSelector
      additionalPrinterColumns:
        - name: Engine
          type: string
          jsonPath: .spec.engineVersion
        - name: Replicas
          type: integer
          jsonPath: .spec.replicas
        - name: Ready
          type: integer
          jsonPath: .status.readyReplicas
        - name: Age
          type: date
          jsonPath: .metadata.creationTimestamp
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              required:
                - engineVersion
                - replicas
                - storageSize
              properties:
                engineVersion:
                  type: string
                  enum:
                    - "14"
                    - "15"
                    - "16"
                replicas:
                  type: integer
                  minimum: 1
                  maximum: 10
                storageSize:
                  type: string
                  pattern: "^[0-9]+(Gi|Ti)$"
            status:
              type: object
              properties:
                readyReplicas:
                  type: integer
                state:
                  type: string
                labelSelector:
                  type: string
```

---

## 5. Custom Printer Columns: Customizing `kubectl get` Output

Notice the `additionalPrinterColumns` block in the manifest above. By default, running `kubectl get <crd>` only outputs `NAME` and `AGE`. 

With custom printer columns configured, `kubectl` formats a clean, informative administrative dashboard directly on the terminal:

```text
$ kubectl get pg -n database
NAME            ENGINE   REPLICAS   READY   AGE
production-db   15       3          3       14d
staging-db      16       1          1       2d
```

---

## 6. Operational Debugging Commands

```bash
# List all registered CRDs in the cluster
kubectl get crds

# Inspect the OpenAPI v3 schema validation rules of a specific CRD
kubectl get crd postgresclusters.database.example.com -o yaml

# Check custom resource instances
kubectl get postgrescluster -A
kubectl get pg -A   # Uses shortNames!

# Validate why a custom resource failed submission
kubectl apply -f bad-postgres-manifest.yaml --validate=true
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (14-Kubernetes-Networking-CNI-and-NetworkPolicies)](../14-Kubernetes-Networking-CNI-and-NetworkPolicies/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - The Kubernetes Operator Pattern and Control Loop →](./02-The-Kubernetes-Operator-Pattern-and-Control-Loop.md) |
