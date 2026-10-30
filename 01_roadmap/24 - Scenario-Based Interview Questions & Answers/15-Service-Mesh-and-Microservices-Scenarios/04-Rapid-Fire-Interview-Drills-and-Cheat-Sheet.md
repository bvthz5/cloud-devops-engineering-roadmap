# Service Mesh & Microservices Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Linkerd Service Mesh Automatic mTLS Certificate Rotation Failure

### 🚨 The Production Scenario
All workloads in a Linkerd-meshed cluster fail cross-service communication with `connection reset by peer`. Linkerd controller logs show: `issuer certificate expired`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Linkerd's trust anchor or identity issuer certificate reached its expiration date. Because automated rotation via Cert-Manager was not configured, the Linkerd identity controller could no longer issue short-lived TLS certificates to proxy sidecars.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check Linkerd certificate validity using `linkerd check --proxy`.
- Generate a new identity issuer certificate and key using the root trust anchor CA.
- Update the `linkerd-identity-issuer` secret in the `linkerd` namespace.
- Restart Linkerd identity controller pods to resume certificate issuance.
- Configure Cert-Manager to manage Linkerd's identity issuer certificate rotation automatically.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Run comprehensive Linkerd proxy and certificate health check
linkerd check --proxy

# Inspect issuer certificate expiration date via step CLI
step certificate inspect /tmp/issuer.crt

# Update Linkerd identity issuer secret
kubectl create secret tls linkerd-identity-issuer --cert=issuer.crt --key=issuer.key -n linkerd --dry-run=client -o yaml | kubectl apply -f -

# Restart Linkerd identity deployment to pick up new certificates
kubectl rollout restart deployment linkerd-identity -n linkerd

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Linkerd uses a hierarchical PKI: a root trust anchor and an identity issuer certificate that signs ephemeral 24-hour proxy certificates. When the issuer certificate expires, the entire mesh collapses. I restore service by issuing a new certificate using our trust anchor, updating the secret, and bouncing the linkerd-identity pod. Long-term, we automate issuer cert renewal via Cert-Manager."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Istio Sidecar Injection Failure & Mutating Webhook Timeout Blocks Pod Scheduling** | `kubectl get mutatingwebhookconfiguration istio-sidecar-injector -o yaml \| grep failurePolicy` | The `istiod` control plane pods were overwhelmed by connection requests or unsch... |
| **Scenario 2: Istio mTLS Strict Mode Outage Breaks Legacy Non-Mesh Communications** | `istioctl authn tls-check my-service.prod.svc.cluster.local` | STRICT mTLS requires both client and server to have Envoy sidecar proxies inject... |
| **Scenario 3: Envoy Proxy Memory Leaks & Sidecar OOMKilled Under High Connection Churn** | `istioctl proxy-config cluster <POD_NAME>.prod` | The application experienced high connection churn (thousands of short-lived TCP ... |
| **Scenario 4: Envoy 503 UC (Upstream Reset) & Connection Termination Race Condition** | `kubectl logs <POD_NAME> -c istio-proxy -n prod \| grep '503 UC'` | A keep-alive timeout race condition: the backend application server (e.g., Node.... |
| **Scenario 5: Istio Circuit Breaking Cascades - Outlier Detection Ejects All Healthy Endpoints** | `istioctl proxy-config endpoints <GATEWAY_POD> -n istio-system --cluster 'outbound\|80\|\|my-service.prod.svc.cluster.local'` | The DestinationRule configured `consecutive5xxErrors: 1` and `maxEjectionPercent... |
| **Scenario 6: Cilium eBPF Service Mesh Drops Pod Traffic Following Kernel Upgrade** | `cilium status --verbose` | The new kernel version introduced changes in eBPF verifier semantics and network... |
| **Scenario 7: Canary Traffic Splitting Leakage - VirtualService Weight Imbalance** | `istioctl proxy-config routes <INGRESS_POD> -n istio-system` | The VirtualService defined traffic weights correctly, but the underlying Kuberne... |
| **Scenario 8: Istio Ingress Gateway TLS Passthrough vs TLS Termination Certificate Mismatch** | `istioctl proxy-config secret <GATEWAY_POD> -n istio-system` | The Istio Gateway was configured with `mode: PASSTHROUGH` expecting the backend ... |
| **Scenario 9: Service Mesh AuthorizationPolicy Accidental Blackhole (Deny-All)** | `kubectl get authorizationpolicy -n prod -o yaml` | In Istio, once any `AuthorizationPolicy` with an `ALLOW` action is defined for a... |
| **Scenario 10: Linkerd Service Mesh Automatic mTLS Certificate Rotation Failure** | `linkerd check --proxy` | Linkerd's trust anchor or identity issuer certificate reached its expiration dat... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Service Mesh & Microservices Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Site Reliability Engineering (SRE) & Chaos Scenarios: Production Incidents & Triage Scenarios →](../16-Site-Reliability-Engineering-and-Chaos-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

