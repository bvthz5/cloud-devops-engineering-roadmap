# 01 - Kubernetes Authentication: X.509, OIDC, and Tokens

## 1. How Kubernetes Authenticates Requests (AuthN Architecture)

**Authentication (AuthN)** is the very first gate an incoming HTTP request must pass when contacting the `kube-apiserver`. If authentication fails, the API server immediately terminates the connection with an `HTTP 401 Unauthorized` response before evaluating authorization permissions or admission webhooks.

A fundamental architectural design principle of Kubernetes is that **the cluster does not maintain a database of human user accounts**. There is no `kind: User` resource stored in `etcd`, nor is there a `kubectl create user` command. Instead, Kubernetes completely offloads human identity verification to external identity systems and relies on cryptographic proofs.

Kubernetes recognizes two distinct classes of users:
1. **Normal Users (Humans):** Software developers, DevOps engineers, and cluster administrators. Identities are verified externally via cryptographic X.509 client certificates or external OpenID Connect (OIDC) enterprise identity providers (such as Okta, Azure AD / Microsoft Entra ID, Keycloak, or Google Workspace).
2. **ServiceAccounts (Machines):** Automated processes, daemon processes, CI/CD runners, and application Pods running inside the cluster. ServiceAccounts are native Kubernetes API resources stored directly in `etcd`.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE KUBERNETES AUTHENTICATION (AuthN) PIPELINE                             |
+---------------------------------------------------------------------------------------------------+

 Incoming Request (HTTPS :6443)
 ┌────────────────────────────────────────────────────────┐
 │ Headers: Authorization: Bearer <JWT_Token>            │
 │ TLS Handshake: Client Certificate presented (mTLS)     │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ kube-apiserver Authentication Pipeline (Evaluates configured Authenticators sequentially)      │
 │                                                                                                 │
 │  1. X.509 Client CA Authenticator   ──► Validates TLS cert against cluster root CA?             │
 │     │ (If valid: extracts CN=User, O=Groups)                                                    │
 │     ▼ (If not present)                                                                          │
 │  2. OIDC Authenticator             ──► Validates JWT signature against IdP JWKS endpoint?       │
 │     │ (If valid: extracts sub/email=User, groups=Groups)                                        │
 │     ▼ (If not present)                                                                          │
 │  3. ServiceAccount Authenticator   ──► Validates Bound Token against TokenRequest API?          │
 │     │ (If valid: system:serviceaccount:<ns>:<name>)                                             │
 │     ▼ (If not present)                                                                          │
 │  4. Anonymous Request Filter       ──► Falls back to user: "system:anonymous", group: "system:unauthenticated"
 └───────────────────────────┬─────────────────────────────────────────────────────────────────────┘
                             │
                             ▼
 Authenticated Identity Object (Passed to Authorization / RBAC):
 ┌────────────────────────────────────────────────────────┐
 │ Username: "jane.doe@enterprise.com"                    │
 │ Groups:   ["developers", "billing-team"]               │
 │ Extra:    { "tenant": ["finance"] }                    │
 └────────────────────────────────────────────────────────┘
```

---

## 2. Authentication Mechanism 1: X.509 Client Certificates

In an X.509 mutual TLS (mTLS) setup, the client presents a cryptographic certificate during the TLS handshake. The API server verifies that the certificate was signed by the cluster's trusted Certificate Authority (`--client-ca-file=/etc/kubernetes/pki/ca.crt`).

The API server parses the certificate's **Subject** fields directly:
- **Common Name (`CN`):** Mapped to the Kubernetes **Username**.
- **Organization (`O`):** Mapped to the Kubernetes **Group** memberships (multiple `O` fields create multiple group associations).

### Real-World OpenSSL Certificate Generation:
```bash
# 1. Generate a private key for developer Jane
openssl genrsa -out jane.key 2048

# 2. Generate a Certificate Signing Request (CSR) with User=jane, Group=platform-engineers
openssl req -new -key jane.key -out jane.csr -subj "/CN=jane/O=platform-engineers/O=developers"

# 3. Sign the CSR using the Kubernetes Cluster Root CA (valid for 1 year)
openssl x509 -req -in jane.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key \
  -CAcreateserial -out jane.crt -days 365
```

### The Critical X.509 Architectural Limitation:
X.509 certificates **cannot be natively revoked** by Kubernetes. Kubernetes does not implement a Certificate Revocation List (CRL) or Online Certificate Status Protocol (OCSP) stapling. If an employee resigns or a private key is leaked, the certificate remains valid until its expiration date unless the cluster administrator regenerates the entire cluster Root CA and re-keys every component. For this reason, enterprise environments avoid X.509 for humans and mandate OIDC.

---

## 3. Authentication Mechanism 2: OpenID Connect (OIDC)

**OpenID Connect (OIDC)** is the enterprise gold standard for human authentication in Kubernetes. It allows Kubernetes to integrate directly with corporate Single Sign-On (SSO) systems.

```text
+---------------------------------------------------------------------------------------------------+
|                        ENTERPRISE OIDC AUTHENTICATION SEQUENCE                                    |
+---------------------------------------------------------------------------------------------------+

 1. Developer runs "kubectl get pods"
 ┌───────────────────┐
 │ Kubectl / Browser │──────────┐ 2. OAuth2 / OIDC Login Request
 └───────────────────┘          ▼
                        ┌────────────────────────────────────────┐
                        │ Enterprise IdP (Okta / Entra ID / ADFS)│
                        │ Prompts for MFA / Corporate Password   │
                        └───────────────┬────────────────────────┘
                                        │
                                        │ 3. Issues signed JWT id_token:
                                        │    { "email": "alice@corp.com", "groups": ["prod-admins"] }
                                        ▼
 4. Kubectl sends HTTP GET with Header:
    Authorization: Bearer <id_token>
 ┌───────────────────┐
 │ kube-apiserver    │
 └─────────┬─────────┘
           │ 5. Validates JWT signature locally using cached IdP public keys (JWKS)
           ▼    Checks expiration (exp) and issuer (iss)
 Identity Established: User="alice@corp.com", Groups=["prod-admins"] ──► Passed to RBAC!
```

### API Server Configuration Flags for OIDC:
```text
--oidc-issuer-url=https://login.microsoftonline.com/tenant-id/v2.0
--oidc-client-id=kubernetes-cluster-client
--oidc-username-claim=email
--oidc-groups-claim=groups
--oidc-ca-file=/etc/ssl/certs/corporate-idp-ca.pem
```

---

## 4. Authentication Mechanism 3: ServiceAccounts and Bound Token Projection

Applications running inside containers authenticate to the API server using **ServiceAccount tokens**.

### Legacy (Pre-1.21) vs Modern Bound Tokens:
- **Legacy ServiceAccount Tokens:** In Kubernetes versions prior to 1.21, ServiceAccount tokens were stored as static plaintext JWTs in `Secret` objects. These tokens **never expired** and were valid cluster-wide. If a container was compromised, attackers could steal the token and use it indefinitely from anywhere on the internet.
- **Modern Bound ServiceAccount Token Projection (v1.21+ standard):** The `TokenRequest` API issues short-lived, cryptographically signed JWT tokens directly to the Kubelet. These tokens:
  1. Have an explicit time-to-live (e.g., 1 hour) and are automatically rotated by the Kubelet.
  2. Are bound to the lifecycle of the specific Pod (if the Pod terminates, the token becomes immediately invalid).
  3. Are audience-bound (`aud`), meaning a token issued for the API server cannot be reused to authenticate against Vault or AWS.

```text
+---------------------------------------------------------------------------------------------------+
|                       PROJECTED BOUND SERVICEACCOUNT TOKEN RUNTIME                                |
+---------------------------------------------------------------------------------------------------+

 Worker Node Kubelet
  ├── Calls TokenRequest API on kube-apiserver
  ├── Receives short-lived signed JWT (valid 1 hour)
  └── Mounts token into container at:
      /var/run/secrets/kubernetes.io/serviceaccount/token
      (Mounted on a secure in-memory tmpfs filesystem; never written to physical disk)
```

---

## 5. Architectural Comparison Matrix: Authentication Methods

| AuthN Method | Primary Use Case | Identity Source | Revocation Capability | Enterprise Production Readiness |
| :--- | :--- | :--- | :--- | :--- |
| **X.509 Client Certs** | Internal Control Plane (`kubelet`, `kube-proxy`) | Local PKI CA | **Impossible** without CA rotation | Not recommended for human developers |
| **OpenID Connect (OIDC)**| Human operators and developers | Corporate IdP (Okta, Entra ID) | **Instant** (revoke user in IdP) | **Mandatory Enterprise Standard** |
| **Bound ServiceAccounts**| In-cluster Pod workloads and controllers | Kubernetes API (`TokenRequest`) | **Automatic** upon Pod death / expiry | **Native Kubernetes Standard** |
| **Webhook Token Auth** | Custom authentication proxies | External HTTP Auth Server | Governed by custom webhook logic | High flexibility for hybrid cloud |
| **Static Token File** | Deprecated legacy clusters | CSV file on master node | Requires API server restart | **Forbidden in Production (Insecure)** |

---

## 6. Verification and Troubleshooting Commands

```bash
# Verify authentication and authorization for the currently configured user
kubectl auth can-i create deployments --namespace default

# Impersonate another user or group to test RBAC rules (requires cluster-admin)
kubectl auth can-i delete pods --as=jane --as-group=developers --namespace billing

# Inspect an X.509 certificate to verify CN and Organization fields
openssl x509 -in /home/user/.minikube/profiles/minikube/client.crt -text -noout | grep -E "Subject:|Issuer:"

# Decode and inspect a projected ServiceAccount JWT payload
kubectl exec -it <pod-name> -- cat /var/run/secrets/kubernetes.io/serviceaccount/token | jq -R 'split(".") | .[1] | @base64d | fromjson'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (11-Auto-Scaling-HPA-VPA-Cluster-Autoscaler)](../11-Auto-Scaling-HPA-VPA-Cluster-Autoscaler/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Role ClusterRole RoleBinding and ClusterRoleBinding →](./02-Role-ClusterRole-RoleBinding-and-ClusterRoleBinding.md) |
