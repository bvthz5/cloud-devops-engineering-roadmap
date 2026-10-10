# 01 - Helm v3 Architecture and Release Lifecycle

## 1. What Is Helm?

**Helm** is the official CNCF-graduated package manager for Kubernetes. In Linux distributions, package managers like `apt` or `yum` automate installing complex software suites by compiling dependencies, configuring configuration files, and managing version lifecycles. Helm fulfills the exact same role for Kubernetes clusters.

In enterprise microservice architectures, a single application rarely consists of a single YAML file. Deploying an application requires coordinating multiple interconnected resources: a `Deployment`, a `Service`, an `Ingress`, an `HPA`, a `ServiceAccount`, multiple `ConfigMaps`, `Secrets`, and `PersistentVolumeClaims`. Managing these raw manifests by hand leads to hardcoded environment drift and error-prone copy-paste templating.

Helm solves this by bundling parameterized Kubernetes manifests into a versioned, reusable archive called a **Helm Chart**, rendering values dynamically using Go templating.

```text
+---------------------------------------------------------------------------------------------------+
|                        RAW YAML SPREAD VS PARAMETERIZED HELM CHART                                |
+---------------------------------------------------------------------------------------------------+

 RAW YAML APPROACH (MANUAL & FRAGILE):
 [ deployment-dev.yaml ]  [ deployment-stage.yaml ]  [ deployment-prod.yaml ]
 (Dozens of duplicated files with hardcoded image tags, replica counts, and ingress domains)

 HELM CHART APPROACH (DYNAMIC & VERSIONED):
 ┌────────────────────────────────────────────────────────┐
 │ Helm Chart Package: "my-microservice:v1.4.0"           │
 │  ├── Chart.yaml (Metadata & Version)                   │
 │  ├── templates/ (Parameterized Go templates)           │
 │  │    ├── deployment.yaml                              │
 │  │    ├── service.yaml                                 │
 │  │    └── ingress.yaml                                 │
 │  └── values.yaml (Default environment variables)       │
 └───────────────────────────┬────────────────────────────┘
                             │
     ┌───────────────────────┼───────────────────────┐
     ▼                       ▼                       ▼
 helm install -f dev.yaml  helm install -f stage.yaml helm install -f prod.yaml
```

---

## 2. Helm v2 vs Helm v3: The Tiller Removal Revolution

The evolution from Helm v2 to Helm v3 represents one of the most critical security and architectural overhauls in cloud-native history.

### The Helm v2 Security Disaster (The "Tiller" Bottleneck)
When Helm was originally designed in 2015, Kubernetes did not possess Role-Based Access Control (RBAC). To execute cluster operations, Helm v2 deployed a centralized server-side daemon inside the cluster called **Tiller**. 
When RBAC was subsequently introduced in Kubernetes 1.6, administrators typically granted Tiller full `cluster-admin` privileges so it could deploy any resource. This introduced a catastrophic security vulnerability:
- Any developer who could communicate with Tiller through the Helm CLI inherited Tiller's full `cluster-admin` root privileges, completely bypassing namespace boundaries and Kubernetes RBAC.
- Tiller was a single point of failure and a high-value attack vector for container escapes.

### Helm v3 Architecture: 100% Client-Side (Tillerless)
Helm v3 completely eradicated Tiller. Helm v3 is a pure **client-side binary**:
1. It does not run any server pods inside the cluster.
2. It communicates directly with `kube-apiserver` via HTTPS REST calls using the engineer's local `~/.kube/config` credentials.
3. Every operation is strictly bounded by the user's personal Kubernetes RBAC permissions.

```text
+---------------------------------------------------------------------------------------------------+
|                        HELM v2 (TILLER) VS HELM v3 (CLIENT-SIDE)                                  |
+---------------------------------------------------------------------------------------------------+

 HELM v2 ARCHITECTURE (INSECURE):
 [ Developer CLI ] ──► [ In-Cluster Tiller Pod (cluster-admin) ] ──► [ kube-apiserver ]
 Result: Massive security vulnerability; bypasses user-level RBAC!

 HELM v3 ARCHITECTURE (SECURE & RBAC-NATIVE):
 [ Developer CLI ] ──────────────────Direct HTTPS──────────────────► [ kube-apiserver ]
 (Authenticates with user's ~/.kube/config; enforced strictly by native RBAC!)
```

---

## 3. State Management: The Secret Storage Architecture

Where does Helm store the state of installed applications?

In Helm v3, release state is stored directly inside the target namespace as **versioned, base64-encoded, gzip-compressed Kubernetes Secrets**:

$$\text{Secret Name: } \text{sh.helm.release.v1.}(\text{release-name})\text{.v}(\text{revision-number})$$

For example, when you run `helm install payment-api ./my-chart -n production`, Helm creates a Secret named:
```text
sh.helm.release.v1.payment-api.v1
```
When you update the deployment via `helm upgrade payment-api ./my-chart`, Helm creates:
```text
sh.helm.release.v1.payment-api.v2
```
Because release history is encapsulated inside the application's own namespace, deleting a namespace automatically cascades and purges all associated Helm release tracking data without leaving orphan state in `kube-system`.

---

## 4. Upgrades: The Three-Way Strategic Merge Patch Algorithm

Helm v3 replaced Helm v2's naive two-way diff with a sophisticated **Three-Way Strategic Merge Patch**.

```text
+---------------------------------------------------------------------------------------------------+
|                     THE 3-WAY STRATEGIC MERGE PATCH RECONCILIATION                                |
+---------------------------------------------------------------------------------------------------+

 Inputs Evaluated by Helm v3 Engine:
 1. Old Manifest (What Helm applied in Revision 1)
 2. Live Cluster State (Current object in etcd - may contain changes made by HPA or operators)
 3. New Manifest (What is declared in the updated Chart / values.yaml)
 
 SCENARIO:
 - Revision 1 declared: replicas = 3
 - Horizontal Pod Autoscaler (HPA) dynamically scaled live cluster to: replicas = 10
 - Engineer runs "helm upgrade" to change image tag to "v2.0.0"
 
 HELM v2 BEHAVIOR (2-Way Diff: Old vs New):
 Overwrote live cluster state! Reset replicas from 10 back to 3, causing a production crash!
 
 HELM v3 BEHAVIOR (3-Way Merge):
 Recognizes replicas = 10 came from an external controller. Upgrades image tag to v2.0.0
 while preserving the live replica count of 10!
```

---

## 5. The Complete Helm Release Lifecycle

A Helm release undergoes a deterministic lifecycle progression:

```text
+---------------------------------------------------------------------------------------------------+
|                           THE HELM RELEASE LIFECYCLE FLOW                                         |
+---------------------------------------------------------------------------------------------------+

 1. helm install ──► 2. Render Engine  ──► 3. Dry-Run / Validation ──► 4. API Commit
    Pulls chart &       Merges values.yaml    Validates rendered YAML      Sends manifests to
    values overrides.   into Go templates.    against k8s OpenAPI spec.   kube-apiserver;
                                                                           saves Release v1 Secret.
                                                                                │
                                                                                ▼
 5. helm rollback ◄── 6. helm upgrade  ◄────────────────────────────────────────┘
    Reverts cluster      Generates 3-way patch;
    to historical        updates objects;
    Secret revision.     creates Release v2 Secret.
```

---

## 6. Operational CLI Commands Cheat Sheet

```bash
# List all active releases across all namespaces
helm list -A

# Inspect rendered YAML without applying it to the cluster (Dry Run)
helm install my-app ./my-chart --dry-run --debug

# View historical release revisions
helm history my-app -n production

# Roll back an unhealthy release to revision 2 immediately
helm rollback my-app 2 -n production

# Extract and inspect the raw values used in an active release
helm get values my-app -n production

# Inspect the actual decrypted release Secret stored in Kubernetes
kubectl get secret -n production -l owner=helm
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (12-RBAC-and-Cluster-Security)](../12-RBAC-and-Cluster-Security/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Helm Chart Directory Structure and Chart yaml →](./02-Helm-Chart-Directory-Structure-and-Chart-yaml.md) |
