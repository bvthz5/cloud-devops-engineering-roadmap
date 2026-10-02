# Kubernetes & Orchestration Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: StatefulSet Pod Stuck in 'Terminating' State during Node Failure

### 🚨 The Production Scenario
A worker node hosting the primary replica of a MongoDB `StatefulSet` crashes. The pod is stuck in `Terminating` for 20 minutes, and Kubernetes will NOT launch a replacement pod on a healthy node.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Unlike Deployments (which are stateless), StatefulSets enforce at-most-one semantic to prevent catastrophic data corruption (split-brain). If a node fails abruptly, Kubernetes cannot determine whether the node is truly dead or just temporarily disconnected. Because the old pod might still be writing to its attached persistent volume (EBS/Persistent Disk), Kubernetes will never create the replacement pod until the old pod is confirmed dead and its `VolumeAttachment` is detached.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Confirm the node is physically powered down or terminated in the cloud provider console.
- Step 2: Verify `VolumeAttachment` status via `kubectl get volumeattachment`.
- Step 3: If and only if the underlying node is confirmed terminated, force delete the stuck pod: `kubectl delete pod <pod> --force --grace-period=0`.
- Step 4: Check if cloud storage volume detachment completed in AWS EBS / GCP PD console.
- Step 5: Verify replacement StatefulSet pod schedules, mounts the existing PV, and syncs.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check current state and hosting node of stuck StatefulSet pod
kubectl get pod -l app=mongodb -o wide

# Inspect storage VolumeAttachment to verify if detachment is blocked on dead node
kubectl get volumeattachment | grep <PVC_NAME>

# Verify in cloud console that physical VM is completely stopped before force deleting
aws ec2 describe-instances --instance-ids <INSTANCE_ID> --query 'Reservations[*].Instances[*].State.Name'

# Force delete pod from etcd only after verifying the host is powered down
kubectl delete pod mongodb-0 --force --grace-period=0

# Confirm PersistentVolume rebinds cleanly to newly created pod instance
kubectl get pvc,pv -l app=mongodb

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "This is by design: StatefulSets guarantee data safety and prevent split-brain. Kubernetes refuses to schedule a replacement StatefulSet pod because it cannot verify whether the crashed node is still writing to the volume. I first verify in the AWS/GCP console that the crashed VM is completely terminated. Only once confirmed do I delete the pod with `--force --grace-period=0`. This releases the VolumeAttachment lock, allowing the cloud provider to attach the EBS volume to a healthy node where the new pod starts."

---

## 📌 Scenario 5: CoreDNS Pods Crashing or CPU Throttled under Cluster Load

### 🚨 The Production Scenario
During high microservices traffic, application pods throw `dial tcp: lookup my-service on 10.96.0.10:53: read: connection refused` or timeout errors.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
All Kubernetes DNS traffic routes through the CoreDNS deployment in `kube-system`. If CoreDNS has insufficient CPU requests/limits, its single-threaded Go runtime experiences severe Linux Completely Fair Scheduler (CFS) quota throttling. In addition, single CoreDNS replicas can get overwhelmed if autoscaling (Cluster Proportional Autoscaler) is misconfigured.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check CoreDNS logs via `kubectl logs -n kube-system -l k8s-app=kube-dns`.
- Step 2: Inspect CoreDNS CPU throttling metrics using Prometheus (`container_cpu_cfs_throttled_seconds_total`).
- Step 3: Remove CPU limits or increase CPU requests on CoreDNS to prevent CFS quota starvation.
- Step 4: Deploy `NodeLocal DNSCache` to handle 90% of DNS lookups locally on each worker node.
- Step 5: Scale CoreDNS replicas using horizontal/proportional autoscaling based on node and core counts.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check live CPU and memory consumption of CoreDNS pods
kubectl top pod -n kube-system -l k8s-app=kube-dns

# Inspect CPU and memory requests/limits on CoreDNS
kubectl get deployment coredns -n kube-system -o yaml | grep -A 5 resources

# Immediately scale CoreDNS replicas to distribute incoming DNS load
kubectl scale deployment coredns -n kube-system --replicas=6

# Review CoreDNS query logs and error rate warnings
kubectl logs -n kube-system -l k8s-app=kube-dns --tail=50

# Verify if NodeLocal DNSCache daemonset is deployed
kubectl get daemonset -n kube-system node-local-dns

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "CoreDNS is the central nervous system of a Kubernetes cluster. When applications report DNS connection refused or timeouts, CoreDNS is usually CPU throttled by Linux CFS quotas. I immediately scale CoreDNS replicas, remove restrictive CPU limits, and verify memory bounds to prevent OOM kills. For permanent resilience, I deploy NodeLocal DNSCache, which runs an in-memory DNS caching agent on every node listening on a link-local IP (`169.254.20.10`), eliminating 90% of CoreDNS network traffic."

---

## 📌 Scenario 6: Silent CPU Throttling on Microservices Despite Low CPU Usage

### 🚨 The Production Scenario
A Java or Go service running on Kubernetes exhibits high p99 latency (500ms) despite reporting average CPU utilization under 30%.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Kubernetes enforces CPU limits using Linux Completely Fair Scheduler (CFS) bandwidth control quotas (`cpu.cfs_quota_us` over a 100ms `cpu.cfs_period_us`). In multi-threaded applications, multiple threads consume the quota concurrently. For instance, a container with a limit of 1 CPU (100ms quota per 100ms period) running 10 concurrent threads consumes the entire 100ms quota in just 10ms of real clock time. The kernel completely throttles the container for the remaining 90ms of the period, causing massive latency spikes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check CPU throttling metrics in Prometheus: `container_cpu_cfs_throttled_periods_total / container_cpu_cfs_periods_total`.
- Step 2: Inspect `/sys/fs/cgroup/cpu/cpu.stat` inside the container.
- Step 3: Remove CPU limits (`resources.limits.cpu`) or significantly increase them, keeping only CPU requests for scheduling.
- Step 4: Align application runtime thread pools (e.g. `GOMAXPROCS` for Go, active processor count for Java) with CPU requests.
- Step 5: Verify p99 latency normalization.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect nr_throttled and throttled_time directly from cgroup virtual filesystem
kubectl exec <POD> -- cat /sys/fs/cgroup/cpu/cpu.stat

# Compare live measured CPU usage against requested limits
kubectl top pod <POD> --containers

# Remove restrictive CPU limits to eliminate kernel CFS throttling
kubectl patch deployment my-service -p '{"spec":{"template":{"spec":{"containers":[{"name":"app","resources":{"limits":{"cpu":null}}}]}}}}'

# Verify GOMAXPROCS or JVM active processor runtime parameters
kubectl exec <POD> -- env | grep -i proc

# Ensure node has sufficient CPU requests headroom
kubectl describe node <NODE> | grep -A 5 'Allocated resources'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "CPU throttling with low average CPU utilization is the classic symptom of Linux CFS quota exhaustion. In multi-threaded runtimes, 10 threads can exhaust a 100ms quota in 10ms, causing the kernel to pause all threads for the remaining 90ms. This is why many leading tech organizations (such as Zalando, Uber, and Buffer) remove CPU limits entirely, relying strictly on CPU requests for scheduling and memory limits for safety. I remove CPU limits and tune `GOMAXPROCS` to match CPU requests."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Kubernetes & Orchestration Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Kubernetes & Orchestration Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

