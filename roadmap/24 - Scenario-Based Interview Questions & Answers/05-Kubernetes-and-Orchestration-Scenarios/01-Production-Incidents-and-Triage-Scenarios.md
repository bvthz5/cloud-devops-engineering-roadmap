# Kubernetes & Orchestration Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Production Pods Stuck in 'Pending' State during Traffic Surge

### 🚨 The Production Scenario
During an autoscaling event, 30 new pods are created but remain stuck in `Pending` state. The API continues to return 503 errors.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Kubernetes `kube-scheduler` cannot schedule the pods onto any available node. Common root causes include: 1) Aggregate CPU/memory `requests` exceed total cluster capacity (`Insufficient cpu/memory`), 2) Rigid pod affinity/anti-affinity or topology spread constraints, 3) Node taints missing tolerations, or 4) PersistentVolumeClaim (PVC) cannot bind to a PV in the node's availability zone.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect scheduler failure events: `kubectl describe pod <pending-pod>`.
- Step 2: Check cluster resource allocations: `kubectl describe nodes | grep -A 8 'Allocated resources'`.
- Step 3: Verify whether cluster autoscaler (Karpenter or Cluster Autoscaler) is triggered or blocked.
- Step 4: Check for storage availability zone mismatches on PVCs (`volumeBindingMode: WaitForFirstConsumer`).
- Step 5: Right-size pod requests, add tolerations, or provision additional worker node pools.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect scheduler diagnostic events to determine exact scheduling roadblock
kubectl describe pod <PENDING_POD> | grep -A 10 'Events:'

# Check available unallocated capacity across all worker nodes
kubectl get nodes -o custom-columns=NAME:.metadata.name,CPU_ALLOC:.status.allocatable.cpu,MEM_ALLOC:.status.allocatable.memory

# Inspect cluster autoscaler logs to verify why new nodes are not launching
kubectl logs -n kube-system -l app=cluster-autoscaler --tail=100

# Locate any PersistentVolumeClaims failing to bind
kubectl get pvc -A | grep -v Bound

# View cluster-wide events in chronological order
kubectl get events --sort-by='.metadata.creationTimestamp' -A | tail -20

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A `Pending` pod indicates the `kube-scheduler` evaluated the cluster nodes through the filtering and scoring predicates and found 0 nodes matching criteria. I immediately run `kubectl describe pod <name>` and look at the `Events` section. 90% of the time it is `0/N nodes available: Insufficient cpu/memory` because CPU requests are set too high or the cluster autoscaler is stuck. I check node allocatable resources, verify Karpenter/Cluster Autoscaler scaling logs, and check PVC zone bindings."

---

## 📌 Scenario 2: HTTP 502 Bad Gateway during Rolling Update of Deployment

### 🚨 The Production Scenario
Every time a new version of an API service is deployed via `kubectl apply`, customers experience a burst of HTTP 502 Bad Gateway errors for 15-30 seconds.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
There are two distinct race conditions during pod termination and startup: 1) The old pod receives `SIGTERM` and shuts down immediately before iptables/IPVS routing rules remove its IP from the Service Endpoints list, routing ongoing requests to a dead pod. 2) The new pod starts, but without a `readinessProbe`, Kubernetes immediately routes traffic to it before the application server (e.g. JVM/Django) has initialized its listening socket.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Add a robust `readinessProbe` with appropriate `initialDelaySeconds` and `periodSeconds`.
- Step 2: Implement a `preStop` lifecycle hook with `sleep 10` to allow endpoint propagation before `SIGTERM`.
- Step 3: Ensure application listens for `SIGTERM` and performs graceful shutdown, draining existing connections.
- Step 4: Configure `terminationGracePeriodSeconds: 45` to grant the application sufficient time to drain.
- Step 5: Configure rolling update strategy (`maxSurge: 25%`, `maxUnavailable: 0`).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Monitor real-time rolling update progress across replicas
kubectl rollout status deployment/my-api

# Verify maxUnavailable is set to 0 to prevent capacity drops
kubectl describe deployment my-api | grep -A 5 'RollingUpdateStrategy'

# Inspect live EndpointSlice propagation as pods transition
kubectl get endpointslices -l kubernetes.io/service-name=my-api

# Test readiness probe endpoint manually
curl -I https://api.mycompany.com/healthz

# Watch pod lifecycle transitions during deployment
kubectl get pods -w

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "502 errors during rolling updates stem from endpoint propagation latency in kube-proxy. When a pod is deleted, the EndpointSlice update and iptables rule removal happen asynchronously. If the pod terminates immediately upon `SIGTERM`, inflight requests hit a closed socket. I solve this by: 1) setting `readinessProbe` so traffic is only sent when ready, 2) setting `maxUnavailable: 0`, and 3) adding a `preStop: exec: command: ['/bin/sh', '-c', 'sleep 15']` hook. This ensures the pod keeps serving while all kube-proxies remove its IP from rotation before SIGTERM is sent."

---

## 📌 Scenario 3: Kubernetes Worker Node Flapping between 'Ready' and 'NotReady'

### 🚨 The Production Scenario
A Kubernetes worker node housing 40 production pods repeatedly flips to `NotReady` every 2 minutes. Pods are evicted, rescheduled, and evicted again, causing massive cluster instability.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Kubernetes node controller marks a node `NotReady` when `kubelet` fails to post node status heartbeats within the `node-monitor-grace-period` (default 40s). Root causes include: 1) High CPU starvation on the host starving `kubelet` or `containerd`, 2) Linux kernel PID exhaustion, 3) D-State hung processes blocking kernel threads, or 4) Ephemeral disk pressure on `/var/lib/kubelet`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Cordon the node immediately to prevent new pods from scheduling: `kubectl cordon <node>`.
- Step 2: Inspect node conditions via `kubectl describe node <node>` (look for MemoryPressure, DiskPressure, PIDPressure).
- Step 3: SSH into node and inspect `kubelet` and `containerd` health via `journalctl -u kubelet -e`.
- Step 4: Verify system daemon resource reservations (`system-reserved` and `kube-reserved`).
- Step 5: Safely drain the node (`kubectl drain <node> --ignore-daemonsets --delete-emptydir-data`) for maintenance or replacement.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Immediately prevent any new pods from being scheduled onto the flapping node
kubectl cordon <NODE_NAME>

# Check for MemoryPressure, DiskPressure, PIDPressure, and KubeletReady false reasons
kubectl describe node <NODE_NAME> | grep -A 10 'Conditions:'

# Inspect kubelet error logs, heartbeats, and API server communication drops
ssh <NODE_IP> 'journalctl -u kubelet -n 100 --no-pager'

# Check for runaway processes consuming 100% CPU and starving system daemons
ssh <NODE_IP> 'top -b -n 1 | head -20'

# Safely evict remaining workloads to healthy nodes before terminating host
kubectl drain <NODE_NAME> --ignore-daemonsets --delete-emptydir-data --force

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When a node flaps `NotReady`, the priority is stopping the cascade. I immediately `kubectl cordon` the node so the scheduler stops thrashing pods onto it. Next, I inspect node conditions (`kubectl describe node`) for DiskPressure or PIDPressure. If kubelet is starved of CPU, it cannot post heartbeats to kube-apiserver. I check `journalctl -u kubelet`, verify `system-reserved` and `kube-reserved` slices to ensure system daemons have dedicated CPU/RAM, and if the host is degraded, I drain and replace it."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Containers & Docker Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../04-Containers-and-Docker-Interview-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Kubernetes & Orchestration Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

