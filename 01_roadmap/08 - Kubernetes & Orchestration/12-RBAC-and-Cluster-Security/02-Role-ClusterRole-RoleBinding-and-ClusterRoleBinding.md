# 02 - Role, ClusterRole, RoleBinding, and ClusterRoleBinding

## 1. What Is Role-Based Access Control (RBAC)?

**Role-Based Access Control (RBAC)** is the native authorization (AuthZ) engine of Kubernetes. After a client has successfully authenticated to the `kube-apiserver`, the authorization layer decides whether that authenticated identity has the right to perform a specific action (`verb`) on a specific API endpoint (`resource`) inside a given scope (`namespace`).

In Kubernetes RBAC:
1. All access is **denied by default** (zero-trust architecture).
2. Permissions are purely **additive** (allow-only rules; there are no explicit "deny" rules).
3. Authorization logic is decoupled into two phases: **defining permissions** (Roles and ClusterRoles) and **assigning permissions to subjects** (RoleBindings and ClusterRoleBindings).

```text
+---------------------------------------------------------------------------------------------------+
|                        THE 4 RBAC PRIMITIVES AND SCOPE TAXONOMY                                   |
+---------------------------------------------------------------------------------------------------+

                          WHAT CAN BE DONE?                       WHO CAN DO IT?
                           (The Permissions)                      (The Assignment)

 NAMESPACE SCOPE:          ┌────────────────┐                     ┌─────────────────────┐
 (Single Namespace)        │      Role      │◄────────────────────│     RoleBinding     │
                           └────────────────┘                     └─────────────────────┘
                                                                             │
                                                                             ▼ (Can also bind to)
 CLUSTER SCOPE:            ┌────────────────┐                     ┌─────────────────────┐
 (Cluster-Wide)            │  ClusterRole   │◄────────────────────│ ClusterRoleBinding  │
                           └────────────────┘                     └─────────────────────┘
```

---

## 2. The 4 Fundamental RBAC Primitives Explained

### 1. `Role` (Namespace Scoped)
A `Role` defines an allowlist of operations on specific resources within a **single namespace**. A Role cannot grant permissions on cluster-wide resources (like Nodes, PersistentVolumes, or StorageClasses) or across multiple namespaces.

### 2. `ClusterRole` (Cluster-Wide Scoped)
A `ClusterRole` defines permissions that apply at the cluster level. It can control:
- Cluster-scoped resources that do not belong to any namespace (e.g., `nodes`, `namespaces`, `persistentvolumes`, `storageclasses`).
- Non-resource REST endpoints (e.g., `/healthz`, `/metrics`, `/version`).
- Standard namespaced resources (e.g., `pods`, `services`) across **all namespaces simultaneously**.

### 3. `RoleBinding` (Namespace Scoped Assignment)
A `RoleBinding` grants the permissions defined in a `Role` or `ClusterRole` to a list of **subjects** (Users, Groups, or ServiceAccounts) **strictly within the RoleBinding's namespace**.

### 4. `ClusterRoleBinding` (Cluster-Wide Assignment)
A `ClusterRoleBinding` grants permissions defined in a `ClusterRole` to subjects across the **entire cluster** (all current and future namespaces).

---

## 3. The Power Architectural Pattern: ClusterRole + RoleBinding

One of the most important architectural patterns in enterprise Kubernetes is **binding a `ClusterRole` using a localized `RoleBinding`**.

If an enterprise has 200 namespaces and wants developers to have read-only access (`view`) inside their specific assigned projects, creating 200 identical `Role` manifests is an unmaintainable operational burden. Instead:
1. Define a single reusable `ClusterRole` once (or use the built-in `view` ClusterRole).
2. Instantiate a localized `RoleBinding` in each team namespace referencing that `ClusterRole`.
3. **The Result:** The developers receive read-only permissions *strictly inside that single namespace*, without gaining any visibility or access to other namespaces!

```text
+---------------------------------------------------------------------------------------------------+
|                   REUSABLE PERMISSION ARCHITECTURE VIA ClusterRole + RoleBinding                  |
+---------------------------------------------------------------------------------------------------+

 ┌────────────────────────────────────────────────────────┐
 │ ClusterRole: "microservice-operator" (Cluster-Scoped)  │
 │ Rules: Get, List, Watch on Deployments, Services, Pods │
 └───────────────────────────┬────────────────────────────┘
                             │ Referenced by multiple localized RoleBindings
         ┌───────────────────┴───────────────────┐
         ▼                                       ▼
 ┌───────────────────────────────┐       ┌───────────────────────────────┐
 │ Namespace: "billing"          │       │ Namespace: "inventory"        │
 │ RoleBinding: "bind-operator"  │       │ RoleBinding: "bind-operator"  │
 │ Subject: Group "billing-devs" │       │ Subject: Group "inv-devs"     │
 └───────────────────────────────┘       └───────────────────────────────┘
 Billing devs only access "billing"; Inventory devs only access "inventory"!
 Zero duplication of permission definitions.
```

---

## 4. Anatomy of an RBAC Policy Rule: API Groups, Resources, and Verbs

An RBAC rule is composed of three mandatory components:

```yaml
rules:
  - apiGroups: ["apps"]                                # 1. The API Group
    resources: ["deployments", "statefulsets"]         # 2. The Resource Type
    verbs: ["get", "list", "watch", "patch", "update"] # 3. The Permitted Verbs
  - apiGroups: [""]                                    # Core API group (pods, configmaps, services)
    resources: ["pods/log"]                            # Subresource syntax
    verbs: ["get"]
  - apiGroups: [""]
    resources: ["configmaps"]
    resourceNames: ["application-config"]              # Granular resource instance filtering
    verbs: ["get", "update"]
```

### Key Dimensions Demystified:
- **`apiGroups`:** The fully qualified API group containing the resource. The core group (`v1`) is denoted by an empty string `[""]`. Groups for other resources include `["apps"]` (Deployments, StatefulSets), `["batch"]` (Jobs, CronJobs), and `["networking.k8s.io"]` (Ingress, NetworkPolicies).
- **Subresources:** Permissions can target specific subresources. For example, `pods/log` allows reading logs without granting access to delete the pod. `pods/exec` grants interactive shell access (`kubectl exec`), which is an extreme security risk.
- **`resourceNames`:** Restricts actions to specific named resource instances, preventing a developer from tampering with other objects of the same type.

---

## 5. Production Manifest: Least-Privilege CI/CD Deployment Role

The following manifest grants a continuous deployment ServiceAccount the exact minimal permissions necessary to execute rolling updates on application workloads:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: github-actions-deployer
  namespace: production
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployment-manager
  namespace: production
rules:
  # Allow viewing pods and fetching container logs
  - apiGroups: [""]
    resources: ["pods", "pods/log"]
    verbs: ["get", "list", "watch"]
  # Allow updating deployment image tags and checking rollout status
  - apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["get", "list", "watch", "patch", "update"]
  # Allow managing services and configmaps
  - apiGroups: [""]
    resources: ["services", "configmaps"]
    verbs: ["get", "list", "watch", "create", "update", "patch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: bind-deployer
  namespace: production
subjects:
  - kind: ServiceAccount
    name: github-actions-deployer
    namespace: production
roleRef:
  kind: Role
  name: deployment-manager
  apiGroup: rbac.authorization.k8s.io
```

---

## 6. Critical Security Vulnerabilities and Privilege Escalation Risks

When designing RBAC policies, security auditors look for specific high-risk permissions that allow unauthorized privilege escalation:

```text
+---------------------------------------------------------------------------------------------------+
|                         HIGH-RISK RBAC PERMISSIONS TO NEVER GRANT BLINDLY                         |
+---------------------------------------------------------------------------------------------------+

 1. "pods/exec" (create):
    Allows interactive shell access into containers. An attacker can compromise the container,
    steal projected ServiceAccount tokens, and pivot across the cluster.

 2. "secrets" (get, list):
    Grants access to all cluster secrets, database passwords, and API keys.

 3. "roles" or "rolebindings" (create, update, bind):
    Allows an unprivileged user to bind the "cluster-admin" role to their own user account,
    escalating their privileges to full cluster takeover!
    (Kubernetes enforces RBAC Escalate checks, but improper configuration can bypass this).

 4. "nodes/proxy" or "pods/proxy":
    Allows bypassing API server admission webhooks and communicating directly with Kubelet endpoints.

 5. Wildcards ("*"):
    Avoid ["*"] in verbs or resources in production; always use explicit, granular lists.
```

---

## 7. Operational RBAC Auditing Commands

```bash
# Check if the current user can delete namespaces
kubectl auth can-i delete namespaces

# Test permissions on behalf of a specific ServiceAccount
kubectl auth can-i patch deployments \
  --as=system:serviceaccount:production:github-actions-deployer \
  --namespace production

# Find all ClusterRoles that grant wildcard access to secrets
kubectl get clusterroles -o json | jq '.items[] | select(.rules[]? | select(.resources[]? == "secrets" and .verbs[]? == "*")) | .metadata.name'

# Audit who has cluster-admin privileges
kubectl get clusterrolebindings -o json | jq '.items[] | select(.roleRef.name == "cluster-admin") | .subjects'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Kubernetes Authentication X509 OIDC and Tokens](./01-Kubernetes-Authentication-X509-OIDC-and-Tokens.md) | [Index](../../../README.md) | [03 - ServiceAccounts and Bound Token Projection →](./03-ServiceAccounts-and-Bound-Token-Projection.md) |
