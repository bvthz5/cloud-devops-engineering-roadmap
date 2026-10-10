# 04 - Resource Requests, Limits, and Quality of Service (QoS)

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Kubernetes resource governance, contrasting Requests versus Limits, compressible (CPU) vs. incompressible (Memory) resources, Linux CFS CPU throttling, OOM Killer mechanics, and Quality of Service (QoS) classes.

---

## 🏛️ 1. What Are Resource Requests and Limits?

In a shared multi-tenant Kubernetes cluster, hundreds of containers run alongside each other on shared physical hardware. If resource governance is omitted:
- A single runaway container with a memory leak can allocate all physical RAM on a worker node, starving neighbouring containers and crashing critical system daemons.
- The `kube-scheduler` would have no mathematical visibility into the hardware requirements of workloads, placing ten memory-intensive workloads onto a single node while other nodes sit idle.

To prevent resource starvation and enable intelligent bin-packing, Kubernetes provides two primary resource directives declared per container in the PodSpec: **`requests`** and **`limits`**.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUESTS VS. LIMITS ARCHITECTURAL MODEL              │
└────────────────────────────────────────────────────────────────────────┘

 [ 1. REQUESTS (Scheduling Contract) ]
   "What does this container NEED to be guaranteed?"
   - Evaluated by: kube-scheduler
   - Used for: Calculating node allocatable capacity and placement.
   - Guaranteed: The node reserves this amount of CPU & RAM exclusively!
         │
         ▼
 [ 2. LIMITS (Runtime Boundary) ]
   "What is the MAXIMUM this container is allowed to consume?"
   - Evaluated by: Linux Kernel cgroups & Completely Fair Scheduler (CFS)
   - Used for: Guardrails against runaway resource consumption.
   - Consequences if breached:
     ├── CPU (Compressible):   THROTTLED (Slowed down, NOT killed)
     └── Memory (Incompressible): OOM KILLED (Exit Code 137, killed immediately!)
```

---

## ⚙️ 2. Compressible (CPU) vs. Incompressible (Memory) Resources

Kubernetes treats compute resources fundamentally differently depending on whether they can be "compressed" or throttled over time:

### 2.1 CPU (Compressible Resource)
- **Measurement Units:** CPU is measured in millicores (`m`) or fractional cores. `1000m` equals `1.0` dedicated CPU core. An allocation of `250m` represents 25% of a single CPU core.
- **Runtime Behavior:** If an application attempts to consume more CPU than its declared `limits.cpu`, the Linux kernel does **not** terminate the process. Instead, the Linux **Completely Fair Scheduler (CFS)** enforces bandwidth quotas (`cfs_quota_us` / `cfs_period_us`). The kernel simply throttles the container's execution time slices, causing the application to execute slower without crashing.

### 2.2 Memory (Incompressible Resource)
- **Measurement Units:** Memory is measured in bytes, typically declared using binary mebibytes (`Mi`) or gibibytes (`Gi`).
- **Runtime Behavior:** Unlike CPU, memory cannot be throttled. A process cannot be told to "use RAM slower". If a container attempts to allocate more RAM than its declared `limits.memory`, the Linux kernel's **Out-Of-Memory (OOM) Killer** intervenes immediately. The kernel sends a forceful `SIGKILL` (Signal 9) to the container process, terminating it with **`Exit Code 137` (OOMKilled)**.

---

## 🎖️ 3. The Three Quality of Service (QoS) Classes

Kubernetes does not require operators to explicitly assign a QoS label. Instead, the API server **automatically calculates and assigns a QoS Class** to every Pod based strictly on the presence, absence, and equality of its containers' requests and limits.

The QoS class dictates the Pod's **Eviction Priority** when a worker node runs out of physical memory and must terminate workloads to protect host stability:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   NODE MEMORY PRESSURE EVICTION ORDER                  │
└────────────────────────────────────────────────────────────────────────┘

 Node runs out of memory! Kubelet must evict pods to survive:
 
 1. BestEffort Pods    ──► TERMINATED FIRST! (oom_score_adj = +1000)
                           Sacrificed immediately to protect cluster.
 
 2. Burstable Pods     ──► TERMINATED SECOND! (oom_score_adj = 100 to 999)
                           Evicted if they exceed their memory requests.
 
 3. Guaranteed Pods    ──► TERMINATED LAST! (oom_score_adj = -997)
                           Preserved at all costs; evicted only if the node
                           is physically collapsing.
```

### 3.1 Guaranteed Class (Highest Reliability)
- **Criteria:** Every container in the Pod (including init containers) must have **both CPU and Memory requests and limits explicitly set, and requests MUST equal limits**:
  $$\text{requests.cpu} == \text{limits.cpu} \quad \text{AND} \quad \text{requests.memory} == \text{limits.memory}$$
- **Linux Kernel Score:** Assigned an `oom_score_adj` of **-997**, making them virtually immune to the OOM killer unless system daemons are under fatal pressure.
- **Recommended For:** Critical stateful databases (PostgreSQL, MySQL), core production payment APIs, and infrastructure gateways.

### 3.2 Burstable Class (Balanced Flexibility)
- **Criteria:** The Pod has at least one container with CPU or Memory requests specified, but it does not meet the strict equality criteria of the Guaranteed class (e.g., `requests.memory < limits.memory`).
- **Behavior:** The Pod is guaranteed its requested allocation, but is permitted to "burst" and consume extra unallocated capacity up to its limits if the node has idle resources.
- **Recommended For:** Standard stateless web microservices that experience variable traffic spikes.

### 3.3 BestEffort Class (Lowest Priority / Ephemeral)
- **Criteria:** Zero containers in the Pod have any CPU or Memory requests or limits configured.
- **Linux Kernel Score:** Assigned the maximum `oom_score_adj` of **+1000**.
- **Behavior:** BestEffort pods are treated as second-class citizens. They consume whatever residual CPU and RAM happens to be idle on the node. The instant a Guaranteed or Burstable pod needs memory, BestEffort pods are killed and evicted immediately without warning.
- **Recommended For:** Non-critical exploratory batch jobs, ad-hoc developer scratch pods, and temporary experiments.

---

## 📝 4. Production Manifest Example: Comparing QoS Configurations

```yaml
# ------------------------------------------------------------------------------
# 1. GUARANTEED POD (requests == limits across all containers)
# ------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: database-guaranteed
spec:
  containers:
  - name: postgres
    image: postgres:16-alpine
    resources:
      requests:
        cpu: "2000m"
        memory: "4Gi"
      limits:
        cpu: "2000m"
        memory: "4Gi"
---
# ------------------------------------------------------------------------------
# 2. BURSTABLE POD (requests < limits)
# ------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: web-api-burstable
spec:
  containers:
  - name: api
    image: mycompany/api:v1.0
    resources:
      requests:
        cpu: "250m"
        memory: "512Mi"
      limits:
        cpu: "1000m"
        memory: "1024Mi"
---
# ------------------------------------------------------------------------------
# 3. BESTEFFORT POD (no requests or limits configured)
# ------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: scratch-besteffort
spec:
  containers:
  - name: batch-worker
    image: busybox
    command: ["sleep", "3600"]
```

---

## ⚠️ 5. The Production CPU Limit Throttling Controversy

In enterprise production engineering, a major operational debate exists regarding whether to configure hard `limits.cpu`:
- **The Problem with CPU Limits:** The Linux CFS quota evaluates CPU consumption over fixed 100ms periods. If a multithreaded application (such as Java or Node.js) spawns 8 threads to process an incoming burst of HTTP requests, it can exhaust its 100ms quota within the first 15ms. The Linux kernel **throttles the application for the remaining 85ms**, causing dramatic, artificial latency spikes (P99 latency degradation) even while host CPU utilization sits at 20%!
- **Modern Enterprise Best Practice:** Many cloud-native enterprises (including Google and Uber) recommend setting **generous CPU requests** to guarantee scheduling, but **omitting CPU limits** (or setting limits very high) to allow bursty threads to execute at full CPU speed without triggering artificial CFS throttling. Memory limits, however, must **always** be strictly enforced to prevent node exhaustion.

---

## 📋 6. Summary and Navigation

Resource requests, limits, and QoS classes provide the mathematical foundation for stable multi-tenant cluster management. By configuring explicit requests for scheduling guarantees, enforcing memory limits against runaway leaks, and understanding the eviction hierarchy, DevOps engineers safeguard mission-critical applications against resource starvation.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Init Containers Sidecars and Ephemeral Containers](./03-Init-Containers-Sidecars-and-Ephemeral-Containers.md) | [Index](../../../README.md) | [05 - Pod Disruption Budgets PDB and Graceful Termination →](./05-Pod-Disruption-Budgets-PDB-and-Graceful-Termination.md) |
