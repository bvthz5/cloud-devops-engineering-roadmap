# Service Mesh & Microservices Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Canary Traffic Splitting Leakage - VirtualService Weight Imbalance

### 🚨 The Production Scenario
A team configures an Istio VirtualService for a 95% v1 and 5% v2 canary deployment. Monitoring indicates that v2 is receiving nearly 40% of production traffic, triggering unexpected capacity overload on the canary pods.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The VirtualService defined traffic weights correctly, but the underlying Kubernetes Service or Gateway lacked session affinity overrides, or an upstream CDN/browser held persistent HTTP/2 connection multiplexing over a single TCP socket routed to a single canary pod.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the VirtualService and DestinationRule configurations via `istioctl`.
- Verify whether HTTP/2 connection pooling is multiplexing hundreds of requests over a single connection established with the canary target.
- Configure DestinationRule load balancing to `LEAST_REQUEST` or `ROUND_ROBIN` at the HTTP stream layer rather than connection layer.
- Ensure consistent hashing / sticky sessions are not hashing a high-volume client IP exclusively to the canary subset.
- Audit traffic distribution metrics in Prometheus (`istio_requests_total`).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify active weighted route table programmed into Ingress Gateway
istioctl proxy-config routes <INGRESS_POD> -n istio-system

# Inspect VirtualService weight allocations
kubectl get virtualservice my-service -n prod -o yaml

# Measure real-time traffic split by destination version
curl -s http://prometheus:9090/api/v1/query?query='sum(rate(istio_requests_total[5m])) by (destination_version)'

# Run automated Istio configuration linter
istioctl analyze -n prod

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Traffic splitting anomalies in HTTP/2 service meshes usually stem from connection multiplexing. In HTTP/2, hundreds of requests stream over one TCP connection. If an Ingress proxy establishes a connection to the canary, all multiplexed streams follow that path. We resolve this by ensuring DestinationRules use stream-level load balancing (like LEAST_REQUEST) and auditing whether sticky session hash keys skewed traffic."

---

## 📌 Scenario 8: Istio Ingress Gateway TLS Passthrough vs TLS Termination Certificate Mismatch

### 🚨 The Production Scenario
An external customer connecting to a secure internal service via Istio Ingress Gateway receives `ERR_SSL_VERSION_OR_CIPHER_MISMATCH`. The client connection fails before HTTP routing begins.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Istio Gateway was configured with `mode: PASSTHROUGH` expecting the backend application to perform TLS termination, but the backend application was configured to serve plaintext HTTP, or the Gateway had `mode: SIMPLE` with an invalid or mismatched TLS secret.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check Gateway resource definition: inspect `tls.mode` (SIMPLE, PASSTHROUGH, MUTUAL).
- Verify Kubernetes Secret containing the TLS certificate and private key in the `istio-system` namespace.
- Inspect Envoy SSL certificate details using `istioctl proxy-config secret`.
- Match domain SANs in the certificate with the `hosts` entry in the Gateway manifest.
- Test direct TLS handshake to the gateway IP using `openssl s_client`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check SSL certificate expiration, SANs, and match status in Ingress Gateway
istioctl proxy-config secret <GATEWAY_POD> -n istio-system

# Test external TLS handshake and inspect presented certificate chain
openssl s_client -connect app.corp.com:443 -servername app.corp.com

# Inspect Gateway tls mode and credentialName
kubectl get gateway my-gateway -n prod -o yaml

# Verify existence of TLS certificate secret in gateway namespace
kubectl get secret -n istio-system | grep my-cert

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When an Ingress Gateway throws cipher or SSL errors, I immediately check 'istioctl proxy-config secret' on the gateway pod. If the mode is PASSTHROUGH, the gateway cannot inspect traffic, and the backend must terminate TLS. If terminating at the gateway, the mode must be SIMPLE, the secret must reside in the gateway's namespace (istio-system), and the certificate SAN must match the incoming Host header."

---

## 📌 Scenario 9: Service Mesh AuthorizationPolicy Accidental Blackhole (Deny-All)

### 🚨 The Production Scenario
An engineer applies an Istio `AuthorizationPolicy` intending to restrict access to the `/admin` path of an internal service. Immediately after application, ALL traffic (including public `/api` endpoints) is rejected with `RBAC: access denied` (HTTP 403).

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
In Istio, once any `AuthorizationPolicy` with an `ALLOW` action is defined for a workload, an implicit default-deny is applied to all requests that do not match the allow rule. Because the policy only allowed `/admin` from a specific source, all regular traffic to `/api` was blocked.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the applied AuthorizationPolicy: identify whether `action: ALLOW` or `action: DENY` was used.
- To restrict a specific path while allowing general traffic, use `action: DENY` targeting `/admin` specifically, rather than `action: ALLOW`.
- Alternatively, add an explicit broad `ALLOW` rule for general paths (`/api/*`) alongside the admin rule.
- Apply the corrected AuthorizationPolicy to restore application access.
- Audit AuthorizationPolicies using `istioctl analyze`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect active AuthorizationPolicies applied to the workload
kubectl get authorizationpolicy -n prod -o yaml

# Inspect Envoy RBAC denial logs and matching policy
kubectl logs <POD_NAME> -c istio-proxy -n prod | grep 'RBAC: access denied'

# Emergency delete of flawed AuthorizationPolicy to restore traffic
kubectl delete authorizationpolicy restrict-admin -n prod

# Lint Istio policies across namespace
istioctl analyze -n prod

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Istio AuthorizationPolicy operates on an implicit deny model: the moment you write a single ALLOW rule for a workload, all other traffic is blocked by default. If your goal is to block access to a specific sub-path like /admin, you must write an explicit DENY rule instead. DENY rules only block matching criteria while allowing all other legitimate traffic to flow freely."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Service Mesh & Microservices Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Service Mesh & Microservices Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

