# Service Mesh & Microservices Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Envoy 503 UC (Upstream Reset) & Connection Termination Race Condition

### 🚨 The Production Scenario
During moderate traffic, clients receive intermittent HTTP 503 errors from Envoy with response flag `UC` (Upstream Connection Termination / Reset). The upstream backend service reports no errors.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A keep-alive timeout race condition: the backend application server (e.g., Node.js, Python Gunicorn, Tomcat) had a keep-alive timeout of 5 seconds, while Envoy's idle timeout was configured to 60 seconds. Envoy attempted to reuse an existing idle TCP connection at the exact instant the backend server closed it, triggering a TCP RST.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Envoy access logs for the `503 UC` response flag (`upstream_reset_before_response_started{connection_termination}`).
- Check application server keep-alive timeout settings.
- Rule of thumb: Upstream application keep-alive timeout must ALWAYS be longer than Envoy's idle timeout (or Envoy's idle timeout must be shorter than backend's).
- Configure Envoy `idleTimeout` in DestinationRule or increase application keep-alive timeout to 65 seconds.
- Implement retry policies in VirtualService for `connect-failure`, `refused-stream`, and `reset`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Filter Envoy access logs for Upstream Connection reset events
kubectl logs <POD_NAME> -c istio-proxy -n prod | grep '503 UC'

# Inspect Envoy upstream endpoint connection status
istioctl proxy-config endpoints <POD_NAME>.prod --cluster 'outbound|8080||my-backend.prod.svc.cluster.local'

# Inspect HTTP response headers for Keep-Alive timeout values
curl -v http://backend:8080/healthz

# Apply VirtualService retry policy for transient connection resets
kubectl apply -f virtualservice-retry.yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Envoy 503 UC is the classic keep-alive mismatch. Envoy thinks an idle socket is open and sends a request just as the backend server closes it due to an idle timeout. To fix this, the backend server's keep-alive timeout must always exceed Envoy's idle timeout. We tune backend keep-alive to 65 seconds and configure an Envoy retry policy on 'reset' to retry cleanly if a race ever occurs."

---

## 📌 Scenario 5: Istio Circuit Breaking Cascades - Outlier Detection Ejects All Healthy Endpoints

### 🚨 The Production Scenario
Following a transient 1-second network spike, an entire backend service becomes unreachable. Istio ingress gateway returns HTTP 503, and Envoy reports that 100% of upstream service pods have been ejected from the load balancing pool.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The DestinationRule configured `consecutive5xxErrors: 1` and `maxEjectionPercent: 100` in its Outlier Detection policy. When a momentary database hiccup caused one 500 error on each pod, Envoy sequentially ejected every single pod in the cluster, completely shutting down the service.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect DestinationRule outlier detection settings: check `consecutive5xxErrors` and `maxEjectionPercent`.
- Immediately patch the DestinationRule to cap `maxEjectionPercent` to a safe threshold (e.g., 20% or 33%) so Envoy can never eject all healthy endpoints.
- Tune `baseEjectionTime` and `interval` to avoid over-aggressive ejections.
- Verify endpoint health restores in Envoy using `istioctl proxy-config endpoints`.
- Monitor circuit breaker ejection metrics in Prometheus (`envoy_cluster_outlier_detection_ejections_enforced_total`).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check healthy vs ejected status of endpoints in Envoy
istioctl proxy-config endpoints <GATEWAY_POD> -n istio-system --cluster 'outbound|80||my-service.prod.svc.cluster.local'

# Inspect DestinationRule outlierDetection and circuit breaker configuration
kubectl get destinationrule my-service -n prod -o yaml

# Cap maxEjectionPercent to 30% to prevent full service blackouts
kubectl patch destinationrule my-service -n prod --type='merge' -p '{"spec":{"trafficPolicy":{"outlierDetection":{"maxEjectionPercent":30}}}}'

# Query active outlier ejections in Prometheus
curl -s http://prometheus:9090/api/v1/query?query=envoy_cluster_outlier_detection_ejections_active

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Circuit breaking is meant to isolate failing pods, but if misconfigured with maxEjectionPercent: 100, a transient spike will eject every single pod and cause an total outage. I mandate that maxEjectionPercent never exceed 30%, ensuring that even during extreme anomalies, at least 70% of pods remain in rotation to serve requests while failing pods cool down."

---

## 📌 Scenario 6: Cilium eBPF Service Mesh Drops Pod Traffic Following Kernel Upgrade

### 🚨 The Production Scenario
After upgrading Linux node kernels from 5.15 to 6.2 across a Kubernetes cluster running Cilium eBPF Service Mesh, cross-node pod-to-pod communication drops completely. Nodes report high packet drops in eBPF maps.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The new kernel version introduced changes in eBPF verifier semantics and network device driver offload behavior (e.g., Geneve/VXLAN checksum offloading or BPF host routing compatibility), causing Cilium eBPF programs to fail verification or drop encapsulated packets.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check Cilium agent status and eBPF program compilation logs using `cilium status` and `cilium-bugtool`.
- Inspect packet drop reasons using `cilium monitor --type drop`.
- Temporarily disable eBPF Host Routing (`bpf-host-routing: false`) or bypass tunneling with native routing.
- Check kernel compatibility matrix with Cilium version and update Cilium to a patch release supporting the 6.x kernel.
- Verify network reachability restores across all nodes.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Cilium agent health, eBPF map status, and BPF features
cilium status --verbose

# Stream real-time eBPF packet drop reasons and offending interface IPs
cilium monitor --type drop

# Inspect eBPF tunnel encapsulation map
cilium bpf tunnel list

# Check for kernel eBPF verifier failure logs
kubectl -n kube-system logs -l k8s-app=cilium --tail=100 | grep -i 'verifier'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cilium operates directly inside Linux kernel eBPF programs. Kernel upgrades can change verifier restrictions or network driver checksum offloads. I run 'cilium monitor --type drop' to capture the exact kernel drop code. If it's a host routing bug, disabling bpf-host-routing temporarily routes traffic via standard Linux networking while we align Cilium and kernel compatibility versions."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Service Mesh & Microservices Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Service Mesh & Microservices Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

