# 04 - Kubernetes API Request Flow: Authentication and Admission

## 1. The 4-Stage Request Pipeline

Whenever `kubectl apply -f pod.yaml` is executed, the HTTP request traverses a strict four-stage processing pipeline within `kube-apiserver` before reaching `etcd`.

```text
HTTP Request (POST /api/v1/namespaces/default/pods)
   │
   ▼
[ 1. AUTHENTICATION (AuthN) ]
   ├── X.509 Client Certificates
   ├── Bearer Tokens (ServiceAccounts, OIDC)
   └── Webhook Token Authentication
   │ (Pass: User / Group identity established)
   ▼
[ 2. AUTHORIZATION (AuthZ) ]
   ├── RBAC (Role / ClusterRole Checks)
   ├── ABAC (Attribute-based)
   └── Node Authorization
   │ (Pass: "User alice has permission 'create' on resource 'pods'")
   ▼
[ 3. MUTATING ADMISSION CONTROLLERS ]
   ├── MutatingAdmissionWebhook (e.g., Istio sidecar injection)
   ├── DefaultStorageClass injector
   └── PodSecurity default mutations
   │ (Modifies object in memory)
   ▼
[ 4. SCHEMA VALIDATION & VALIDATING ADMISSION ]
   ├── OpenAPI Schema & Type checks
   ├── ValidatingAdmissionWebhook (e.g., Kyverno / OPA Gatekeeper)
   └── PodSecurityStandards (Privileged / Baseline / Restricted)
   │ (Strict Pass / Reject)
   ▼
[ PERSISTENCE IN ETCD ]
```

---

## 2. Optimistic Concurrency Control

Kubernetes avoids distributed database locks by implementing **Optimistic Concurrency Control (OCC)** using the `metadata.resourceVersion` field:
1. Client $A$ reads Pod version `12450`.
2. Client $B$ reads Pod version `12450`.
3. Client $A$ updates Pod; `kube-apiserver` commits to etcd and increments `resourceVersion` to `12451`.
4. Client $B$ submits update with old version `12450`.
5. `kube-apiserver` immediately rejects Client $B$'s request with `409 Conflict` (`the object has been modified; please apply your changes to the latest version and try again`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Etcd Distributed Storage](./03-Etcd-Distributed-Storage-Quorum-and-Raft.md) | [README](./README.md) | [05 - HA Control Plane Topologies](./05-High-Availability-Control-Plane-Topologies.md) |
