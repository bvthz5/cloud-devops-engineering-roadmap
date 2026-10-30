# Kubernetes & Orchestration Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Cluster Out of IP Addresses (CNI IPAM Subnet Exhaustion)

### 🚨 The Production Scenario
In an AWS EKS or Azure AKS cluster, new pods fail to launch with `FailedCreatePodSandBox: failed to allocate for range: no IP addresses available in range`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Cloud-native CNIs (like AWS VPC CNI or Azure CNI) assign secondary private IP addresses directly from the VPC subnet to every pod. If the VPC subnet CIDR block is too small (e.g. `/24` with only 250 IPs), a cluster with multiple worker nodes and daemonsets exhausts the entire subnet, leaving zero IPs for new application pods.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect available IPs in the VPC subnet via cloud console or CLI.
- Step 2: In AWS EKS, enable Custom Networking with a dedicated non-routable secondary CIDR block (e.g., `100.64.0.0/16` CGNAT).
- Step 3: Enable prefix delegation (`ENABLE_PREFIX_DELEGATION=true`) on AWS VPC CNI to allocate `/28` IP blocks (16 IPs) per ENI slot.
- Step 4: Right-size node pools and migrate nodes to larger subnets.
- Step 5: Verify new pods obtain IPs and transition to `Running`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check remaining unassigned IP addresses in target VPC subnet
aws ec2 describe-subnets --subnet-ids <SUBNET_ID> --query 'Subnets[*].AvailableIpAddressCount'

# Enable VPC CNI prefix delegation to multiply pod IP capacity per node
kubectl set env daemonset aws-node -n kube-system ENABLE_PREFIX_DELEGATION=true

# Configure warm IP prefix buffer to reduce ENI allocation churn
kubectl set env daemonset aws-node -n kube-system WARM_PREFIX_TARGET=1

# Check maximum allowable pod count per node after prefix delegation
kubectl get nodes -o custom-columns=NAME:.metadata.name,PODS:.status.allocatable.pods

# Confirm CNI network interface binding success
kubectl describe pod <STUCK_POD> | grep -i cni

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Subnet exhaustion in AWS VPC CNI happens because every pod consumes a real VPC IP address. When subnets run dry, pods get stuck in `ContainerCreating`. I remediate this by: 1) Enabling Prefix Delegation (`ENABLE_PREFIX_DELEGATION=true`), which assigns `/28` IPv4 blocks (16 IPs per slot) instead of individual IPs, vastly increasing density per EC2 node, and 2) Implementing EKS Custom Networking with a secondary 100.64.0.0/16 CGNAT CIDR dedicated solely to pod networking."

---

## 📌 Scenario 8: Ingress Controller Returning 503 Service Temporarily Unavailable

### 🚨 The Production Scenario
An Nginx Ingress or AWS Load Balancer Controller returns intermittent HTTP 503 errors during traffic fluctuations, while backend pods appear healthy.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When an Ingress controller routes traffic directly to Pod IPs, it synchronizes its internal routing table with Kubernetes `Endpoints` or `EndpointSlices`. If pods crash or restart faster than the Ingress controller can update its configuration or if readiness probe thresholds are too tight, the Ingress controller forwards requests to stale or unready pod IPs.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect Ingress controller logs for upstream connection errors: `kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx`.
- Step 2: Verify whether endpoints match actual running pod IPs: `kubectl get endpoints <service-name>`.
- Step 3: Increase readiness probe stability (`failureThreshold: 3`, `periodSeconds: 10`).
- Step 4: Enable Ingress controller keepalive connections to upstreams (`upstream-keepalive-connections`).
- Step 5: Ensure application pod graceful shutdown allows ongoing connections to finish.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Ingress controller logs for specific upstream connection failures
kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx --tail=100 | grep '503'

# Compare active service endpoint IPs with live pod IPs
kubectl get endpoints <SERVICE_NAME>

# Verify backend service port and path routing mappings
kubectl describe ingress <INGRESS_NAME>

# Verify all backend pods report Running and 1/1 Ready
kubectl get pods -l app=backend -o wide

# Bypass DNS and directly query Ingress controller proxy layer
curl -v -H 'Host: api.mycompany.com' http://<INGRESS_LB_IP>/healthz

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "An Ingress 503 indicates the proxy cannot establish a connection to any upstream pod IP. This happens when the EndpointSlice list becomes empty or contains IPs of pods that are shutting down. I check `kubectl get endpoints` to verify active targets, audit Ingress controller logs to see if it's hitting connection limits, and configure `readinessProbes` with proper `failureThreshold` and `initialDelaySeconds` to prevent flapping pods from being exposed prematurely."

---

## 📌 Scenario 9: RBAC Authorization Denial Blocking Microservices from Kubernetes API

### 🚨 The Production Scenario
A newly deployed monitoring agent or custom operator fails on startup with `User "system:serviceaccount:default:default" cannot list resource "pods" in API group "" at the cluster scope`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Kubernetes enforces strict Role-Based Access Control (RBAC). By default, pods mount the `default` ServiceAccount in their namespace, which has zero cluster permissions. Applications that interact with the Kubernetes API (like Prometheus, cert-manager, or custom controllers) require an explicit `ServiceAccount`, bound to a `Role` (namespaced) or `ClusterRole` (cluster-wide) via a `RoleBinding` or `ClusterRoleBinding`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check which ServiceAccount the pod is using in its manifest (`spec.serviceAccountName`).
- Step 2: Check current permissions with `kubectl auth can-i`: `kubectl auth can-i list pods --as=system:serviceaccount:<namespace>:<sa>`.
- Step 3: Create a dedicated `ServiceAccount` and least-privilege `ClusterRole` defining required `apiGroups`, `resources`, and `verbs`.
- Step 4: Bind the role to the service account using a `ClusterRoleBinding`.
- Step 5: Redeploy the pod with `serviceAccountName: <my-sa>`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check if a specific ServiceAccount has permission to list pods
kubectl auth can-i list pods --as=system:serviceaccount:monitoring:prometheus-sa

# Verify existing role bindings associated with the service account
kubectl get clusterrolebindings | grep <SERVICE_ACCOUNT>

# Inspect verbs (get, list, watch) and resources granted in the ClusterRole
kubectl describe clusterrole <ROLE_NAME>

# List all ServiceAccounts present in the target namespace
kubectl get sa -n <NAMESPACE>

# Print complete matrix of API permissions granted to a service account
kubectl auth can-i --list --as=system:serviceaccount:default:my-app

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "This is standard RBAC policy: pods default to an unprivileged ServiceAccount. I verify permissions using `kubectl auth can-i list pods --as=system:serviceaccount:<ns>:<sa>`. I resolve this following least privilege: define a dedicated `ServiceAccount`, create a `ClusterRole` granting strictly the needed verbs (`get`, `list`, `watch`) on specific API groups, attach them via `ClusterRoleBinding`, and assign `spec.serviceAccountName` in the pod deployment."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Kubernetes & Orchestration Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Kubernetes & Orchestration Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

