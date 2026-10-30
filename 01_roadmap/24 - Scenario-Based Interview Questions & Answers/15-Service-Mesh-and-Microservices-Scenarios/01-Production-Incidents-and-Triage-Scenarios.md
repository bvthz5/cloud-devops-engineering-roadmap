# Service Mesh & Microservices Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Istio Sidecar Injection Failure & Mutating Webhook Timeout Blocks Pod Scheduling

### 🚨 The Production Scenario
During an autoscaling event, new microservice pods remain stuck in `Pending` state. Pod descriptions show: `Internal error occurred: failed calling webhook 'sidecar-injector.istio.io': post https://istiod.istio-system.svc:443: context deadline exceeded`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The `istiod` control plane pods were overwhelmed by connection requests or unscheduled due to resource starvation. Because the mutating webhook was configured with `failurePolicy: Fail`, any timeout in contacting istiod blocked the Kubernetes API server from scheduling any pods.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect `istiod` pod status and resource utilization using `kubectl top pods -n istio-system`.
- Scale up `istiod` replicas horizontally and increase CPU/memory requests.
- If immediate cluster recovery is required, temporarily adjust `failurePolicy: Ignore` on the webhook configuration, or exclude non-critical namespaces.
- Ensure `istiod` pods have High Priority / PriorityClass and PodDisruptionBudgets to prevent eviction.
- Verify network reachability between API server and istio-system namespace.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect webhook failurePolicy (Fail vs Ignore)
kubectl get mutatingwebhookconfiguration istio-sidecar-injector -o yaml | grep failurePolicy

# Inspect istiod control plane error logs
kubectl logs -l app=istiod -n istio-system --tail=100

# Horizontally scale istiod control plane
kubectl scale deployment istiod -n istio-system --replicas=3

# Check istiod memory and CPU consumption
kubectl top pods -n istio-system

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When a mutating webhook has failurePolicy: Fail and its backend hangs, the entire Kubernetes cluster scheduling halts. I prioritize keeping istiod resilient by running minimum 3 replicas with anti-affinity, high PriorityClasses, and tuning webhook timeouts. During an emergency, changing failurePolicy to Ignore temporarily restores pod scheduling while istiod is restored."

---

## 📌 Scenario 2: Istio mTLS Strict Mode Outage Breaks Legacy Non-Mesh Communications

### 🚨 The Production Scenario
After applying a cluster-wide `PeerAuthentication` resource enforcing `STRICT` mTLS, several legacy cron jobs, third-party monitoring agents, and internal services fail to connect to microservices with `SSL routines:ssl3_read_bytes:tlsv1 alert decrypt error`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
STRICT mTLS requires both client and server to have Envoy sidecar proxies injecting and verifying mutual TLS certificates. Legacy workloads lacking an Istio sidecar attempted plaintext TCP/HTTP handshakes, which the server Envoy proxy rejected by policy.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Change the global `PeerAuthentication` mode from `STRICT` to `PERMISSIVE` to immediately restore legacy connectivity.
- In `PERMISSIVE` mode, Envoy accepts both plaintext and mTLS traffic while logging peer identities.
- Audit all services communicating with the cluster to identify workloads lacking Istio sidecars.
- Inject sidecars into unmeshed workloads or configure explicit DestinationRules and PeerAuthentication exceptions per namespace or port.
- Transition back to `STRICT` mode namespace by namespace once all workloads are sidecar-enabled.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check mTLS configuration status between mesh workloads
istioctl authn tls-check my-service.prod.svc.cluster.local

# List all PeerAuthentication policies and modes
kubectl get peerauthentication -A

# Switch namespace mTLS to Permissive mode for safe migration
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: prod
spec:
  mtls:
    mode: PERMISSIVE
EOF

# Check sync status between istiod and all Envoy sidecars
istioctl proxy-status

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Enforcing STRICT mTLS globally without validating client readiness is a classic service mesh outage. I recover immediately by shifting the PeerAuthentication policy to PERMISSIVE mode. PERMISSIVE mode accepts both mTLS and plaintext traffic, allowing us to inspect Istio telemetry, identify unmeshed clients, inject sidecars systematically, and promote namespaces to STRICT mode one by one."

---

## 📌 Scenario 3: Envoy Proxy Memory Leaks & Sidecar OOMKilled Under High Connection Churn

### 🚨 The Production Scenario
Application pods run smoothly, but their `istio-proxy` sidecar containers restart every few hours with exit code 137 (OOMKilled). When the sidecar restarts, active client requests are terminated with 503 Service Unavailable.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The application experienced high connection churn (thousands of short-lived TCP connections per second). Envoy buffers connection state and dynamic cluster configurations. With default sidecar memory limits (e.g., 256MB), high connection concurrency and excessive cluster discovery services (CDS/EDS) exhausted the sidecar cgroup memory limit.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Envoy memory stats using `istioctl dashboard proxy` or Prometheus metric `container_memory_working_set_bytes`.
- Increase sidecar resource requests and limits in the pod annotations or global Istio injection template.
- Enable connection keep-alive in client microservices to eliminate wasteful TCP connection handshakes.
- Apply `Sidecar` resources in each namespace to restrict Envoy configuration discovery to only necessary dependencies, drastically reducing Envoy memory footprint.
- Enable Envoy concurrency tuning (`concurrency: 2`).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect total number of clusters loaded into Envoy memory
istioctl proxy-config cluster <POD_NAME>.prod

# Check for presence of namespace-scoped Istio Sidecar resources
kubectl get sidecar -n prod

# Inspect Envoy sidecar termination logs
kubectl logs <POD_NAME> -c istio-proxy -n prod --tail=100

# Open live Envoy administrative dashboard to view memory allocations
istioctl dashboard envoy <POD_NAME>.prod

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "By default, every Envoy sidecar downloads configuration for every single service in the entire Kubernetes cluster, consuming massive RAM. I solve sidecar OOMKilled by implementing Istio 'Sidecar' custom resources that restrict discovery egress strictly to services that workload actually depends on. This reduces Envoy's cluster index size by 90% and keeps sidecar memory stable under 80MB."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← DevSecOps, Identity & Secrets Management Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../14-DevSecOps-Identity-and-Secrets-Management-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Service Mesh & Microservices Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

