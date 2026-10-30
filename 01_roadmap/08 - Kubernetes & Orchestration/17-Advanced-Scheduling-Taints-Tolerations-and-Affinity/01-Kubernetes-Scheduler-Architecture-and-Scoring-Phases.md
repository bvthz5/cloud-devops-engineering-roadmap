# 01 - Kubernetes Scheduler Architecture and Scoring Phases

## 1. What Is the Kubernetes Scheduler (`kube-scheduler`)?

The **`kube-scheduler`** is the control plane component responsible for watching newly created Pods that lack an assigned node (`spec.nodeName == ""`) and selecting the single best worker node in the cluster to host each Pod.

A common misconception is that the scheduler physically places containers onto worker machines. In reality, the scheduler is an algorithmic decision engine. Once it computes the optimal node, it executes an HTTP `POST` request to the API server creating a lightweight **`Binding`** object. This binding updates the Pod's `spec.nodeName` field in `etcd`. The `kubelet` running on that designated worker node observes the update via its watch loop and proceeds to pull container images and run the Pod.

Since Kubernetes 1.19, the scheduler is built on an extensible, plugin-based architecture called the **Scheduling Framework**.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE KUBERNETES SCHEDULING FRAMEWORK LIFECYCLE                              |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ SCHEDULING CYCLE (Synchronous, Sequential Pod Evaluation)                                       │
 │                                                                                                 │
 │  [ Unschedulable Pod in activeQ ]                                                               │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  1. QueueSort: Sorts pods by PriorityClass, creation timestamp, and retry backoff.             │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  2. PreFilter / Filter (Predicates):                                                            │
 │     Hard elimination pass: inspects every node in cluster.                                      │
 │     Removes nodes lacking CPU/RAM, possessing untolerated taints, or with port collisions.      │
 │                 │                                                                               │
 │                 ├──► If 0 Nodes survive ──► Trigger PostFilter (Preemption / Eviction Engine)   │
 │                 ▼ (1 or more candidate nodes survive)                                           │
 │  3. PreScore / Score (Priorities):                                                              │
 │     Soft ranking pass: assigns a numerical score (0 to 100) to each surviving candidate node.   │
 │     Evaluates resource balance, image locality, pod affinity, and topology spread.              │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  4. NormalizeScore: Weights and combines all plugin scores into a single final composite score. │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  5. Reserve: Optimistically locks resources in scheduler local memory cache (assumed state).    │
 └─────────────────┼───────────────────────────────────────────────────────────────────────────────┘
                   │
                   ▼ (Handoff to Asynchronous Goroutine)
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ BINDING CYCLE (Asynchronous Execution - Does Not Block Next Pod Scheduling)                     │
 │                                                                                                 │
 │  6. Permit: Allows delaying binding (e.g., Gang Scheduling / batch all-or-nothing co-scheduling)│
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  7. PreBind: Executes prerequisite operations (e.g., dynamic network or storage attachments).   │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  8. Bind: Writes the Binding API object (spec.nodeName = "worker-02") to kube-apiserver!        │
 │                 │                                                                               │
 │                 ▼                                                                               │
 │  9. PostBind: Cleans up internal reservations and emits Prometheus metrics.                     │
 └─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Deep Dive: The Filter Phase (Hard Constraints)

During the **Filter phase** (historically called *Predicates*), the scheduler evaluates candidate nodes against a series of mandatory Boolean checks. If a node fails even a single filter check, it is immediately discarded from consideration for that pod.

### Key Core Filter Plugins:
1. **`NodeResourcesFit`:** Verifies whether the worker node has sufficient free **Allocatable** CPU, memory, and ephemeral storage to accommodate the Pod's `resources.requests`.
2. **`NodePorts`:** Checks if any container in the Pod requests a specific `hostPort` that is already bound by another container running on that node.
3. **`NodeName`:** If the Pod specifies an explicit `spec.nodeName`, this filter eliminates all nodes except the exact requested machine.
4. **`NodeAffinity`:** Evaluates `nodeSelector` and `nodeAffinity.requiredDuringSchedulingIgnoredDuringExecution`. Nodes lacking required labels are filtered out.
5. **`TaintsTolerations`:** Checks if the node has any `Taints` (e.g., `dedicated=gpu:NoSchedule`). If the pod does not have matching `Tolerations`, the node is eliminated.
6. **`VolumeBinding`:** For PVCs using `volumeBindingMode: WaitForFirstConsumer`, verifies if the storage provider can actually provision the disk in the node's specific cloud availability zone.

---

## 3. Deep Dive: The Score Phase (Soft Ranking)

During the **Score phase** (historically called *Priorities*), all candidate nodes that passed the Filter phase are evaluated by scoring plugins. Each plugin assigns a score from **0 to 100**. The scheduler then multiplies each score by an administrator-configured plugin weight and sums them:

$$\text{FinalNodeScore} = \sum_{i=1}^{n} \left( \text{PluginScore}_i \times \text{Weight}_i \right)$$

The node with the highest composite score is selected. If multiple nodes tie with the exact same highest score, the scheduler selects one at random using round-robin.

```text
+---------------------------------------------------------------------------------------------------+
|                        SCORING PLUGINS: NodeResourcesBalancedAllocation                           |
+---------------------------------------------------------------------------------------------------+

 SCORING OBJECTIVE: Avoid resource fragmentation!
 If Node A has 90% CPU used and 10% Memory used, placing another CPU-heavy pod there exhausts CPU
 while stranding 90% of Memory.
 
 Formula evaluates variance between CPU and Memory allocation ratios:
 Variance = | (NodeAllocatedCPU / NodeTotalCPU) - (NodeAllocatedMem / NodeTotalMem) |
 
 High Variance (e.g., 80% CPU vs 20% RAM) ──► LOW SCORE! (Resource fragmentation)
 Low Variance  (e.g., 60% CPU vs 60% RAM) ──► HIGH SCORE! (Balanced resource consumption)
```

### Key Core Scoring Plugins:
1. **`NodeResourcesBalancedAllocation`:** Favors nodes where the resulting CPU and Memory utilization ratios will be balanced, minimizing fragmented idle capacity.
2. **`ImageLocality`:** Awards higher scores to nodes that **already have the container image pre-pulled** in their local container runtime cache. This significantly reduces pod startup latency by eliminating multi-gigabyte network image downloads.
3. **`NodeAffinityScoring`:** Evaluates `preferredDuringSchedulingIgnoredDuringExecution` (soft affinity rules), awarding extra points to nodes that match preferred labels.
4. **`PodTopologySpread`:** Awards points to nodes in zones or racks that currently host fewer replicas of the application, ensuring maximum fault tolerance across failure domains.

---

## 4. Multi-Profile Scheduling Configuration

Modern Kubernetes allows defining multiple distinct scheduler configurations within a single `kube-scheduler` process. Different teams or workloads can target different profiles by specifying `spec.schedulerName`:

```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
leaderElection:
  leaderElect: true
profiles:
  # Profile 1: Default general-purpose scheduler
  - schedulerName: default-scheduler
  # Profile 2: Batch compute scheduler favoring nodes with pre-pulled images
  - schedulerName: batch-scheduler
    plugins:
      score:
        disabled:
          - name: NodeResourcesBalancedAllocation
        enabled:
          - name: ImageLocality
            weight: 10
```

To bind a Pod to the specialized batch scheduler profile, declare `schedulerName` in the Pod spec:
```yaml
spec:
  schedulerName: batch-scheduler
  containers:
    - name: ml-worker
      image: tensor-batch:v2.1
```

---

## 5. Operational Debugging: Why Is My Pod Stuck in `Pending`?

When a Pod remains stuck in `Pending` due to scheduling constraints, examine the event log:

```bash
kubectl describe pod <pending-pod-name>
```

### Common Scheduling Event Failures Decoded:
- `0/12 nodes available: 12 Insufficient cpu.`
  - **Meaning:** Every node in the cluster passed taints and affinity checks, but none had enough unallocated allocatable CPU to satisfy the pod's `resources.requests.cpu`.
- `0/10 nodes available: 3 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 7 node(s) didn't match PodTopologySpread.`
  - **Meaning:** 3 master nodes were rejected due to control-plane taints, and the remaining 7 worker nodes violated high-availability topology spread rules.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (16-Kubernetes-Troubleshooting-and-Debugging)](../16-Kubernetes-Troubleshooting-and-Debugging/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Node Selection NodeName NodeSelector and NodeAffinity →](./02-Node-Selection-NodeName-NodeSelector-and-NodeAffinity.md) |
