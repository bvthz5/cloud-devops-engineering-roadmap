# 04 - Kubernetes API Request Flow: Authentication and Admission

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Kubernetes API Server request processing pipeline, exploring Authentication (AuthN), Authorization (AuthZ), Mutating Admission Webhooks, Schema Validation, Validating Admission Webhooks, and Optimistic Concurrency Control.

---

## 🏛️ 1. What Is the Kubernetes API Request Pipeline?

Every administrative action performed against a Kubernetes cluster—whether executed interactively by a human engineer using `kubectl`, triggered automatically by a GitOps engine like ArgoCD, or initiated by an in-cluster controller—originates as an HTTP REST request directed to the `kube-apiserver`.

Because the API server is the sole gatekeeper to the cluster's persistent `etcd` database, it cannot simply write incoming JSON or YAML data directly to disk. Allowing unvetted, malformed, or malicious requests to reach `etcd` could instantly compromise cluster security, break networking, or trigger cascading node outages.

To prevent this, the `kube-apiserver` processes every incoming HTTP request through a **strict, non-negotiable multi-stage pipeline**. If a request fails at any point in this sequence, execution is halted immediately and an HTTP error code is returned to the client.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   KUBERNETES API REQUEST LIFECYCLE                     │
└────────────────────────────────────────────────────────────────────────┘

 Incoming Client Request: POST /api/v1/namespaces/production/pods
                            │
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 1. AUTHENTICATION (AuthN)                                            │
 │    "Who are you?"                                                    │
 │    - X.509 Client Certificates (Subject CN=alice, O=developers)       │
 │    - ServiceAccount Bearer Tokens (JWT with bound audience)          │
 │    - OpenID Connect (OIDC via Okta, Google, Azure AD)                │
 │    - Webhook Token Authenticator                                     │
 └──────────────────────────┬───────────────────────────────────────────┘
                            │ (Identity Established: User "alice", Group "devs")
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 2. AUTHORIZATION (AuthZ)                                             │
 │    "Are you allowed to perform this action?"                         │
 │    - Node Authorization (Secures kubelet boundaries)                 │
 │    - Role-Based Access Control (RBAC: Roles & ClusterRoles)          │
 │    - Webhook Authorization                                           │
 └──────────────────────────┬───────────────────────────────────────────┘
                            │ (Approved: User "alice" can CREATE pods in "production")
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 3. MUTATING ADMISSION CONTROLLERS                                    │
 │    "Can we modify or inject defaults into this object?"              │
 │    - Built-in Mutators (DefaultStorageClass, LimitRanger)             │
 │    - MutatingAdmissionWebhooks (Istio sidecar injection, Vault agent)│
 └──────────────────────────┬───────────────────────────────────────────┘
                            │ (Object mutated in memory: Added envoy sidecar)
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 4. SCHEMA VALIDATION & VALIDATING ADMISSION                          │
 │    "Is this object structurally legal and compliant with policy?"    │
 │    - OpenAPI v3 Schema Validation (Types, required fields)           │
 │    - Pod Security Standards (Privileged / Baseline / Restricted)     │
 │    - ValidatingAdmissionWebhooks (OPA Gatekeeper, Kyverno policies)  │
 └──────────────────────────┬───────────────────────────────────────────┘
                            │ (Strict Approval: Conforms to all enterprise policies)
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 5. PERSISTENCE IN ETCD                                               │
 │    - Serialized into Protocol Buffers (protobuf)                     │
 │    - Optimistic Concurrency Control (resourceVersion verified)       │
 │    - Committed to /registry/pods/production/my-pod                   │
 └──────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 2. Detailed Breakdown of the Pipeline Stages

### Stage 1: Authentication (AuthN) — "Who Are You?"
- **Purpose:** Examines the HTTP request headers, TLS handshake certificates, or bearer tokens to extract the client's identity: **Username**, **User ID (UID)**, and associated **Groups**.
- **Crucial Architectural Realization:** **Kubernetes does not have a "User" database or User resource object.** You cannot run `kubectl get users`. Kubernetes assumes human users and corporate identities are managed by an external identity provider (IdP).
- **Core Authentication Strategies:**
  1. **X.509 Client Certificates:** Used by cluster administrators and internal control plane components. The API server verifies the client certificate against its internal Certificate Authority (`ca.crt`). The certificate's **Common Name (CN)** becomes the Kubernetes username, and the **Organization (O)** fields become group memberships.
  2. **ServiceAccount Tokens:** Used by software processes running inside pods. Modern Kubernetes uses **Bound ServiceAccount Tokens**—time-limited JSON Web Tokens (JWTs) cryptographically signed by the API server and bound to a specific pod and audience.
  3. **OpenID Connect (OIDC):** The enterprise standard for human engineers. Connects the API server to corporate identity systems like Okta, Keycloak, or Microsoft Azure AD. The client presents an OAuth ID Token, which the API server validates using the IdP's public keys.

### Stage 2: Authorization (AuthZ) — "Are You Allowed to Do This?"
- **Purpose:** Determines whether the authenticated identity possesses permission to execute the requested verb (`get`, `list`, `create`, `update`, `delete`, `watch`) on the target resource (`pods`, `services`, `deployments`) within the specified namespace.
- **Evaluation Mechanism:** The API server evaluates configured authorization modules in sequential order. The first module that explicitly approves the request grants access. If all modules pass without approval, the request is denied with an **`HTTP 403 Forbidden`**.
- **Primary Authorization Modes:**
  1. **Role-Based Access Control (RBAC):** The universal standard. Compares the user against `Roles` (namespace-scoped) and `ClusterRoles` (cluster-scoped) linked via `RoleBindings` and `ClusterRoleBindings`.
  2. **Node Authorization:** A specialized authorization module specifically designed for kubelets. It ensures a worker node can only access Secrets, ConfigMaps, and Pods that are scheduled to run on that specific node, preventing a compromised node from accessing secrets across the entire cluster.

### Stage 3: Mutating Admission Controllers — "Modify the Object"
- **Purpose:** Inspects and optionally mutates (modifies) the incoming object before it is committed to storage.
- **Sequential Execution:** Mutating controllers run in sequence. A change made by one mutator is visible to subsequent mutators.
- **Real-World Use Cases:**
  1. **Automatic Sidecar Injection:** When a pod is submitted with the label `istio-injection=enabled`, the Istio Mutating Webhook intercepts the PodSpec in memory and automatically injects an Envoy proxy container and shared volume mounts before the pod is created.
  2. **Default Storage Class Injection:** If a PersistentVolumeClaim omits `storageClassName`, the `DefaultStorageClass` mutator injects the cluster's default storage class name.

### Stage 4: Object Schema Validation & Validating Admission — "Enforce Policy"
- **Purpose:** Enforces strict structural syntax and compliance with enterprise security policies. Unlike mutating webhooks, validating webhooks **cannot modify the object**; they can only approve or reject it.
- **Parallel Execution:** To optimize API server throughput, validating admission controllers are evaluated **concurrently in parallel**. If any validator rejects the object, the entire transaction is rejected.
- **Key Enforcers:**
  1. **OpenAPI v3 Schema Validation:** Verifies that field names exist, data types match (e.g. integer vs string), and required fields are populated.
  2. **Pod Security Standards (PSS):** Built-in admission controller replacing legacy PodSecurityPolicies. Enforces three security tiers (`privileged`, `baseline`, `restricted`).
  3. **External Policy Engines (OPA Gatekeeper & Kyverno):** Validating admission webhooks that enforce corporate guardrails (e.g., rejecting any container image pulling from unapproved registries, requiring resource limits, or banning root execution).

### Stage 5: Persistence & Optimistic Concurrency Control (OCC)
Once all validation checks pass, the API server serializes the object into efficient **Protocol Buffers (protobuf)** and writes it to `etcd`.

To prevent write collisions without using heavy database row locks, Kubernetes uses **Optimistic Concurrency Control (OCC)** governed by `metadata.resourceVersion`:
1. Client A and Client B both read a Pod at `resourceVersion: 500`.
2. Client A submits an update. The API server writes the update to `etcd` and increments the version to `501`.
3. Client B submits an update still referencing the old `resourceVersion: 500`.
4. The API server immediately rejects Client B's write with **`HTTP 409 Conflict`**:
   ```text
   Operation cannot be fulfilled on pods "my-pod": the object has been modified;
   please apply your changes to the latest version and try again.
   ```
5. Client B must re-read version `501`, re-apply its modifications, and resubmit.

---

## 🛠️ 3. Verification & Diagnostic Commands

```bash
# 1. Test your current identity's permissions using the 'can-i' command
kubectl auth can-i create deployments --namespace production

# 2. Impersonate a different user or serviceaccount to verify RBAC policies
kubectl auth can-i delete pods --as=developer-alice --namespace staging
kubectl auth can-i get secrets --as=system:serviceaccount:default:my-app

# 3. Inspect the full HTTP REST interaction and admission response via verbose flags
kubectl apply -f pod.yaml -v=8

# 4. View active admission webhooks registered in the cluster
kubectl get mutatingwebhookconfigurations
kubectl get validatingwebhookconfigurations
```

---

## 📋 4. Summary and Navigation

The Kubernetes API request processing pipeline guarantees that only authenticated, authorized, structurally sound, and policy-compliant configurations ever reach persistent storage. By separating authentication, authorization, mutation, and validation into clean architectural phases, Kubernetes delivers enterprise security and bulletproof state integrity.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Etcd Distributed Storage Quorum and Raft](./03-Etcd-Distributed-Storage-Quorum-and-Raft.md) | [Index](../../../README.md) | [05 - High Availability Control Plane Topologies →](./05-High-Availability-Control-Plane-Topologies.md) |
