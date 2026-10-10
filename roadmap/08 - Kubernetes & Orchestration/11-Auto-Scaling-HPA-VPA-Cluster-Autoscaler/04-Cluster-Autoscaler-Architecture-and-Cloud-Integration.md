# 04 - Cluster Autoscaler Architecture and Cloud Integration

## 1. What Is the Kubernetes Cluster Autoscaler?

The **Kubernetes Cluster Autoscaler (CA)** is a standalone control loop that automatically adjusts the physical size of a Kubernetes cluster by provisioning new worker virtual machines (VMs) or terminating idle nodes.

While the Horizontal Pod Autoscaler (HPA) operates exclusively at the **workload layer** (adding or removing Pod containers), Cluster Autoscaler operates at the **infrastructure layer** (adding or removing physical or virtual computing nodes). 

HPA and Cluster Autoscaler work in tandem: when high traffic causes HPA to generate 30 new Pods, the existing worker nodes quickly exhaust their allocatable CPU and RAM. The newly created Pods transition into a `Pending` state. Cluster Autoscaler detects these pending pods, calculates how many additional worker nodes are required, and calls cloud provider infrastructure APIs to scale up the underlying node pools.

```text
+---------------------------------------------------------------------------------------------------+
|                        CLUSTER AUTOSCALER SCALE-UP WORKFLOW                                       |
+---------------------------------------------------------------------------------------------------+

 1. HPA scales workload from 5 to 30 Pods
 ┌────────────────────────────────────────────────────────┐
 │ Pods 1 to 12 scheduled on existing worker nodes        │
 │ Pods 13 to 30 enter "Pending" state (Insufficient CPU) │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 2. Cluster Autoscaler Core Loop (Polls every 10 seconds)
 ┌────────────────────────────────────────────────────────┐
 │ Detects 18 Pending Pods with FailedScheduling events.  │
 │ Runs internal simulated scheduler against known        │
 │ Node Group templates to find which pool fits the pods. │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 3. Cloud Provider API Invocation
 ┌────────────────────────────────────────────────────────┐
 │ Calls Cloud Auto-Scaling API:                          │
 │ AWS: Auto Scaling Group (ASG) DesiredCapacity: 4 ──► 7 │
 │ GCP: Managed Instance Group (MIG) resize               │
 │ Azure: Virtual Machine Scale Set (VMSS) scale          │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 4. Worker Nodes Boot, Register with API Server, & Pods Scheduled
 ┌────────────────────────────────────────────────────────┐
 │ 3 New VMs boot ──► Kubelet registers Ready state       │
 │ Kube-Scheduler binds all 18 Pending Pods!              │
 └────────────────────────────────────────────────────────┘
```

---

## 2. The Scale-Down Architecture and Node Drain Safeguards

Scale-down is significantly more hazardous than scale-up because terminating a node forcibly destroys active TCP connections, local caches, and running transactions. To prevent service disruption, Cluster Autoscaler follows a rigorous, multi-step decommissioning protocol:

```text
+---------------------------------------------------------------------------------------------------+
|                        CLUSTER AUTOSCALER SCALE-DOWN PROTOCOL                                     |
+---------------------------------------------------------------------------------------------------+

 1. Underutilization Scan:
    CA calculates node utilization: sum(Pod CPU Requests) / Allocatable Node CPU.
    If utilization < 50% for sustained period (default: 10 minutes), marked as "Candidate".
         │
         ▼
 2. Eviction Feasibility Simulation:
    CA simulates moving all non-DaemonSet pods on the candidate node to other cluster nodes.
    Can the rest of the cluster absorb these pods without exceeding 100% capacity?
         │ YES
         ▼
 3. Safety Check: Are Any Pods Non-Evictable?
    CA checks for conditions that forbid node deletion:
    - Pod has annotation: cluster-autoscaler.kubernetes.io/safe-to-evict: "false"
    - Pod is not managed by a controller (standalone/naked pod)
    - Pod has local storage (emptyDir with data or hostPath)
    - Pod eviction would violate a PodDisruptionBudget (PDB)
    - Pod is in kube-system namespace without PDB
         │ NONE FOUND
         ▼
 4. Graceful Cordon and Drain:
    CA marks node Unschedulable (Cordon) ──► Evicts pods via Eviction API ──► Calls Cloud API to Terminate VM!
```

---

## 3. Production Deployment Configuration (AWS EKS ASG Auto-Discovery)

To deploy Cluster Autoscaler in a cloud environment such as AWS, the controller uses auto-discovery tags to detect which Auto Scaling Groups it is permitted to manipulate:

### Required Cloud Infrastructure Tags (on the AWS ASG):
- `k8s.io/cluster-autoscaler/enabled` = `true`
- `k8s.io/cluster-autoscaler/<my-cluster-name>` = `owned`

### Production Controller Deployment Manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    app: cluster-autoscaler
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
      annotations:
        prometheus.io/scrape: 'true'
        prometheus.io/port: '8085'
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
        - image: registry.k8s.io/autoscaling/cluster-autoscaler:v1.28.2
          name: cluster-autoscaler
          resources:
            limits:
              cpu: 100m
              memory: 600Mi
            requests:
              cpu: 100m
              memory: 600Mi
          command:
            - ./cluster-autoscaler
            - --v=4
            - --stderrthreshold=info
            - --cloud-provider=aws
            - --skip-nodes-with-local-storage=false
            - --expander=least-waste
            - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/my-production-cluster
            - --balance-similar-node-groups
            - --scale-down-enabled=true
            - --scale-down-delay-after-add=10m
            - --scale-down-unneeded-time=10m
            - --scale-down-utilization-threshold=0.5
            - --max-node-provision-time=15m
```

---

## 4. Node Allocation Expander Strategies

When multiple node groups can satisfy a pending pod's requirements, the `--expander` flag determines which pool Cluster Autoscaler chooses:

| Expander Strategy | Decision Logic | Ideal Use Case |
| :--- | :--- | :--- |
| **`random` (Default)** | Selects a valid node group randomly. | Testing and development clusters. |
| **`least-waste`** | Selects the node group that will have the lowest unallocated idle CPU/RAM percentage after scheduling the pods. | General production workloads aiming to minimize cost. |
| **`most-pods`** | Selects the node group that can schedule the highest absolute number of pending pods right now. | Clusters with heterogeneous pod shapes. |
| **`price`** | Selects the node group with the lowest hourly financial cost (requires GCE or AWS pricing engine). | Cost-optimized production clusters. |
| **`priority`** | Uses a user-defined ConfigMap with explicit priority tiers (e.g. prioritize Spot instances first; fallback to On-Demand only if Spot is unavailable). | Enterprise cost-saving architectures leveraging Spot instances. |

---

## 5. Failure Modes and Operational Troubleshooting

### Scenario A: Pending Pods Fail to Trigger Scale-Up
- **Symptom:** Pods remain stuck in `Pending` for 30 minutes. Cluster size never expands.
- **Root Cause:** The pending pods have conflicting scheduling constraints that no node group can satisfy. For example, a pod requests 64GB of RAM, but the largest ASG in the cluster uses `t3.medium` instances (4GB RAM).
- **Verification:**
  ```bash
  kubectl logs -n kube-system -l app=cluster-autoscaler --tail=100
  ```
  *Log output:* `pod default/huge-workload-xyz doesn't fit in any node group`
- **Resolution:** Add an instance type to the cluster with sufficient hardware capacity.

### Scenario B: Nodes Refuse to Scale Down
- **Symptom:** Cluster Autoscaler reports nodes as underutilized, but never terminates them.
- **Root Cause:** A pod running on the underutilized node lacks a controller or has an active `PodDisruptionBudget` with `minAvailable: 100%`, blocking the eviction API.
- **Verification Command:**
  ```bash
  kubectl describe configmap cluster-autoscaler-status -n kube-system
  ```
  The status ConfigMap documents exactly why each node was rejected for scale-down.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Vertical Pod Autoscaler VPA Modes and Recommender](./03-Vertical-Pod-Autoscaler-VPA-Modes-and-Recommender.md) | [Index](../../../README.md) | [05 - Karpenter Next Generation Just in Time Node Autoscaling →](./05-Karpenter-Next-Generation-Just-in-Time-Node-Autoscaling.md) |
