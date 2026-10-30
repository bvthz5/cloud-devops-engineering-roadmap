# 04 - Taints and Tolerations: Node Cordon and Drain Mechanics

## 1. What Are Taints and Tolerations?

In Kubernetes cluster administration, **Taints and Tolerations** work together to ensure that sensitive or dedicated worker nodes do not accept inappropriate workloads.

While **NodeAffinity** attracts a Pod toward a specific set of nodes (a "pull" model), a **Taint** allows a node to **repel** all Pods (a "push-away" model) unless a Pod possesses an explicit, matching **Toleration**.

A classic real-world analogy is a security badge:
- A worker node with expensive NVIDIA H100 GPUs applies a **Taint** ("Restricted GPU Zone").
- General web frontend and microservice pods lack the required badge and are repelled.
- Machine learning training pods carry a **Toleration** in their pod manifest, allowing them to pass through the gate and run on the GPU hardware.

```text
+---------------------------------------------------------------------------------------------------+
|                        TAINTS AND TOLERATIONS: THE REPULSION PARADIGM                             |
+---------------------------------------------------------------------------------------------------+

 Worker Node with Taint: "workload=ml-training:NoSchedule"
 ┌────────────────────────────────────────────────────────┐
 │ Node-01 (GPU Rig)                                      │
 │ Taint: key="workload", value="ml-training",            │
 │        effect="NoSchedule"                             │
 └───────────────────────────┬────────────────────────────┘
                             │
         ┌───────────────────┴───────────────────┐
         ▼                                       ▼
 ┌───────────────────────────────┐       ┌───────────────────────────────┐
 │ Standard Nginx Pod            │       │ PyTorch Training Pod          │
 │ Tolerations: None             │       │ Tolerations:                  │
 │                               │       │ - key: "workload"             │
 │                               │       │   operator: "Equal"           │
 │                               │       │   value: "ml-training"        │
 │                               │       │   effect: "NoSchedule"        │
 ├───────────────────────────────┤       ├───────────────────────────────┤
 │ RESULT: REPELLED!             │       │ RESULT: ADMITTED!             │
 │ Scheduler skips this node.    │       │ Scheduler binds to Node-01!   │
 └───────────────────────────────┘       └───────────────────────────────┘
```

---

## 2. The 3 Taint Effects Explained

A Taint is defined by a `key`, an optional `value`, and a mandatory `effect`:

$$\text{Taint Schema: } \text{key}=\text{value}:\text{effect}$$

| Taint Effect | Scheduling Behavior | Running Pod Behavior |
| :--- | :--- | :--- |
| **`NoSchedule`** | **Hard Reject:** The scheduler will not place any new pod on this node unless it has a matching toleration. | **Untouched:** Pods already running on the node before the taint was applied continue running without interruption. |
| **`PreferNoSchedule`**| **Soft Preference:** The scheduler attempts to place new pods on untainted nodes. If no untainted nodes have free capacity, it falls back to this node. | **Untouched:** Existing running pods are left unaffected. |
| **`NoExecute`** | **Hard Reject:** New pods without toleration cannot be scheduled onto this node. | **Immediate Eviction!** Any currently running pod lacking a matching toleration is **immediately evicted and terminated**! |

---

## 3. Automated System Taints and `tolerationSeconds`

Kubernetes automatically applies system taints to worker nodes when physical hardware failures or network partitions occur:
- `node.kubernetes.io/not-ready:NoExecute` (Node Kubelet stopped heartbeating)
- `node.kubernetes.io/unreachable:NoExecute` (Node is partitioned from control plane)
- `node.kubernetes.io/memory-pressure:NoSchedule` (Node RAM is nearly exhausted)
- `node.kubernetes.io/disk-pressure:NoSchedule` (Node root filesystem is full)

By default, Kubernetes injects tolerations into every Pod with a built-in **`tolerationSeconds: 300`** countdown for `not-ready` and `unreachable`:

```yaml
tolerations:
  - key: "node.kubernetes.io/not-ready"
    operator: "Exists"
    effect: "NoExecute"
    tolerationSeconds: 300
  - key: "node.kubernetes.io/unreachable"
    operator: "Exists"
    effect: "NoExecute"
    tolerationSeconds: 300
```

### The Architectural Purpose of `tolerationSeconds`:
If a worker node experiences a brief 15-second network hiccup or Kubelet restart, evicting hundreds of pods immediately would trigger an expensive, unnecessary cluster-wide rescheduling storm. With `tolerationSeconds: 300`, Kubernetes waits **5 minutes**. If the node recovers within 5 minutes, running pods are never disturbed. If the node remains dead past 300 seconds, the pods are gracefully evicted and rescheduled onto healthy nodes.

---

## 4. Production Node Maintenance: Cordon and Drain Mechanics

During operating system kernel patching, security upgrades, or physical hardware decommissioning, cluster operators must drain worker nodes without causing application downtime.

```text
+---------------------------------------------------------------------------------------------------+
|                        ZERO-DOWNTIME NODE DRAIN MAINTENANCE LIFECYCLE                             |
+---------------------------------------------------------------------------------------------------+

 STEP 1: Cordon Node
 Command: kubectl cordon worker-01
 Action: Marks node spec.unschedulable = true.
         Active pods stay running, but Kube-Scheduler is blocked from assigning any new pods.
       │
       ▼
 STEP 2: Drain Node
 Command: kubectl drain worker-01 --ignore-daemonsets --delete-emptydir-data
 Action: 1. Verifies PodDisruptionBudgets (PDB) to ensure minimum application quorum is preserved.
         2. Sends SIGTERM to evict non-DaemonSet pods.
         3. Controller managers replace evicted pods on other healthy worker nodes.
       │
       ▼
 STEP 3: Execute Physical Maintenance
 Action: Reboot VM, apply Linux kernel patches, upgrade Kubelet binary, upgrade container runtime.
       │
       ▼
 STEP 4: Uncordon Node
 Command: kubectl uncordon worker-01
 Action: Marks node spec.unschedulable = false. Node resumes accepting new pods from scheduler!
```

---

## 5. Production Manifests and CLI Management

### Applying and Removing Taints via CLI:
```bash
# Apply a NoSchedule taint to a GPU worker node
kubectl taint nodes worker-gpu-01 dedicated=gpu:NoSchedule

# Apply a NoExecute taint to evict unauthorized pods
kubectl taint nodes worker-gpu-01 environment=specialized:NoExecute

# Remove a taint (append a minus sign '-' to the end)
kubectl taint nodes worker-gpu-01 dedicated=gpu:NoSchedule-
```

### Pod Toleration Manifest (Matching GPU Workload):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pytorch-distributed-trainer
  namespace: ml-workloads
spec:
  containers:
    - name: trainer
      image: pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime
      resources:
        limits:
          nvidia.com/gpu: "2"
  tolerations:
    # Matches dedicated=gpu:NoSchedule exactly
    - key: "dedicated"
      operator: "Equal"
      value: "gpu"
      effect: "NoSchedule"
    # Matches any taint with key "environment" regardless of value
    - key: "environment"
      operator: "Exists"
      effect: "NoExecute"
```

---

## 6. Critical Operational Pitfalls

### Pitfall 1: Draining Fails Due to `emptyDir` Volumes
- **Symptom:** `kubectl drain` errors out with: `cannot delete Pods with local storage: emptyDir`.
- **Reason:** By default, `drain` protects local ephemeral data. If an application uses an `emptyDir` for scratch caching, `drain` refuses to terminate the pod to prevent accidental data loss.
- **Resolution:** Provide the explicit flag `--delete-emptydir-data` after confirming the data is ephemeral.

### Pitfall 2: Draining Deadlocks on Broken PodDisruptionBudgets
- **Symptom:** `kubectl drain` hangs indefinitely with `Cannot evict pod as it would violate the pod's disruption budget`.
- **Reason:** A deployment with 1 replica has a PDB with `minAvailable: 1`. Kubernetes will never allow that single replica to be evicted.
- **Resolution:** Temporarily scale up the deployment to 2 replicas or adjust the PDB before draining.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Pod Affinity and Pod Anti Affinity Co location Rules](./03-Pod-Affinity-and-Pod-Anti-Affinity-Co-location-Rules.md) | [Index](../../../README.md) | [05 - Topology Spread Constraints Multi AZ High Availability →](./05-Topology-Spread-Constraints-Multi-AZ-High-Availability.md) |
