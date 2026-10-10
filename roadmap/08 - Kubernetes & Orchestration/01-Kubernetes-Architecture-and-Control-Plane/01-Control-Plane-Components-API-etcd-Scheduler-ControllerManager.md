# 01 - Control Plane Components: API, etcd, Scheduler, and Controller Manager

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Kubernetes Control Plane, exploring the history of container orchestration, `kube-apiserver`, `etcd` distributed consensus, `kube-scheduler` two-phase placement, `kube-controller-manager` reconciliation loops, and disaster resilience.

---

## 🏛️ 1. History of Kubernetes & Container Orchestration

To understand why Kubernetes is engineered the way it is, one must understand the distributed systems crises at Google that preceded its creation.

| Technology / Milestone | Inventor(s) & Organization | Year | The Real Problem / Crisis That Triggered It | Real-World Impact |
| :--- | :--- | :---: | :--- | :--- |
| **Google Borg** | **Google Systems Infrastructure Team** (Urs Hölzle, Abhishek Verma et al.) | **2003–2004** | Google operated hundreds of thousands of bare-metal servers running Search, Indexing, and Gmail. When hardware servers died, engineers could not manually SSH to reboot tasks. Borg automated bin-packing, dynamic scheduling, and self-healing. | Proved that containerized orchestrators could manage global web-scale infrastructure autonomously. |
| **Google Omega** | **Google Cluster Management Team** (Brian Grant, Malte Schwarzkopf et al.) | **2013** | Borg had a monolithic centralized scheduler that became a lock contention bottleneck as clusters expanded. Omega introduced a shared-state transaction database architecture. | Formed the direct architectural predecessor to Kubernetes' declarative API Server + `etcd` model. |
| **The Kubernetes Project ("Project Seven")** | **Joe Beda, Brendan Burns, Craig McLuckie** (Google, with Tim Hockin, Brian Grant) | **June 2014 (v1.0 July 2015)** | With Docker exploding in 2013, developers had isolated containers but no standard open-source way to run 10,000 containers across 50 servers with zero downtime. Google wrote Kubernetes in Go based on 10+ years of Borg/Omega experience. | Became the de-facto operating system of the cloud and the foundation of modern cloud-native computing. |
| **Donation to CNCF** | **Google, Linux Foundation & 200+ Partners** | **July 2015** | To avoid corporate vendor lock-in, Google donated Kubernetes to the newly founded Cloud Native Computing Foundation (CNCF), ensuring neutral governance across AWS, Azure, Google Cloud, and Red Hat. | Standardized the global cloud-native ecosystem with vendor neutrality. |

### 🕰️ Visual Timeline: The Evolution of Kubernetes
```text
 2003/2004 ──────── 2013 ──────── 2014 ──────── 2015 ──────── 2018 ──────── Present
     │                │             │             │             │              │
Google Borg      Google Omega   DockerCon     Kubernetes    K8s Graduates   Managed K8s
Google Internal  Shared-State   Announcement  v1.0 & CNCF   from CNCF       Ubiquity
Containerized    Transaction    "Project      Donation      Enterprise      (EKS, AKS,
Datacenters      Architecture   Seven"        by Google     Standard        GKE & Edge)
```

---

## ⚙️ 2. High-Level Control Plane Architecture

The **Kubernetes Control Plane** (historically called the Master Node) represents the brain and nervous system of the cluster. It makes global decisions about workload placement, detects and responds to node failures, maintains the declared desired state, and exposes the administrative API to users and continuous delivery pipelines.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        KUBERNETES CONTROL PLANE                        │
└────────────────────────────────────────────────────────────────────────┘

        ┌─────────────────────────┐       ┌──────────────────────────────┐
        │     kube-scheduler      │       │   kube-controller-manager    │
        │  (Two-Phase Placement)  │       │    (Reconciliation Loops)    │
        └────────────┬────────────┘       └──────────────┬───────────────┘
                     │                                   │
                     │ (Watches unscheduled pods)        │ (Watches resource diffs)
                     ▼                                   ▼
        ┌────────────────────────────────────────────────────────────────┐
        │                         kube-apiserver                         │
        │   - Serves REST API on port 6443 (HTTPS)                       │
        │   - Authentication (mTLS, JWT, Webhooks)                       │
        │   - Authorization (RBAC, Node, Webhook)                        │
        │   - Admission Controllers (Mutating & Validating)              │
        │   - Optimistic Concurrency Control (resourceVersion)           │
        └───────────────────────────────┬────────────────────────────────┘
                                        │
                                        │ (Exclusive direct gRPC read/write)
                                        ▼
        ┌────────────────────────────────────────────────────────────────┐
        │                             etcd                               │
        │   - Distributed, consistent Key-Value store (Raft consensus)   │
        │   - Single source of truth under /registry/ hierarchy          │
        │   - MVCC revisions and transaction logs                        │
        └────────────────────────────────────────────────────────────────┘
                                        │
                                        │ (Encrypted mTLS gRPC Communication)
                                        ▼
┌────────────────────────────────────────────────────────────────────────┐
│                            WORKER NODE POOL                            │
│                                                                        │
│  ┌──────────────────────────────────┐┌───────────────────────────────┐ │
│  │ Node 01                          ││ Node 02                       │ │
│  │ [kubelet] ──► [CRI / containerd] ││ [kubelet] ──► [containerd]    │ │
│  │ [kube-proxy] (iptables / IPVS)   ││ [kube-proxy] (iptables/IPVS)  │ │
│  └──────────────────────────────────┘└───────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🔬 3. Deep Architectural Breakdown of Control Plane Components

### 3.1 `kube-apiserver`: The Central Brain and Gateway
- **What it is:** The primary front-end component of the control plane that exposes the Kubernetes REST API over HTTPS (default port `6443`).
- **Why it is needed:** Without a centralized API gateway, components would have to synchronize state directly across disparate nodes, creating distributed lock contention and unpredictable security perimeters.
- **Working Mechanism:**
  1. **Strict Isolation of `etcd`:** The `kube-apiserver` is the **only component in the entire cluster that communicates directly with `etcd`**. The scheduler, controller manager, kubelets, and kubectl never touch `etcd`; they communicate exclusively via the API server.
  2. **Stateless Scalability:** The API server holds no local state in memory. Multiple API server replicas can run concurrently behind a Layer 4 (HAProxy, AWS NLB) or Layer 7 load balancer.
  3. **Optimistic Concurrency Control (OCC):** Every object stored in Kubernetes possesses a `metadata.resourceVersion` integer. When a client attempts to update an object, the API server verifies that the client's version matches the current version in `etcd`. If another client updated the object in the interim, the update is rejected with an `HTTP 409 Conflict`, preventing write collisions without database table locks.
  4. **The Complete Request Processing Pipeline:**
     ```text
     Incoming Request ──► Authentication ──► Authorization (RBAC) ──► Mutating Webhooks ──► Schema Validation ──► Validating Webhooks ──► etcd Persistence
     ```

### 3.2 `etcd`: The Immutable Distributed Source of Truth
- **What it is:** A distributed, strongly consistent key-value store created by CoreOS that implements the **Raft consensus algorithm**.
- **Why it is needed:** Kubernetes requires a distributed database that guarantees **Linearizable Reads and Writes**. If two managers attempt to schedule workloads during a network partition, the cluster must guarantee zero split-brain data corruption.
- **Working Mechanism:**
  1. **Raft Consensus & Quorum:** An `etcd` cluster operates with an odd number of members ($N = 3, 5, 7$). To accept any write transaction, a strict majority **Quorum** of nodes must agree:
     $$\text{Quorum} = \left\lfloor \frac{N}{2} \right\rfloor + 1$$
     A 3-node cluster can tolerate the failure of 1 node. A 5-node cluster can tolerate the failure of 2 nodes.
  2. **Multi-Version Concurrency Control (MVCC):** `etcd` never mutates data in place. When a key is updated, `etcd` appends a new revision while preserving previous revisions in its underlying **bbolt B+tree storage engine**. This allows the API server to perform efficient "Watch" operations, streaming historical change streams to controllers.
  3. **Key Hierarchy:** Kubernetes organizes cluster state hierarchically under the `/registry` prefix:
     ```text
     /registry/pods/<namespace>/<pod-name>
     /registry/deployments/<namespace>/<deployment-name>
     /registry/secrets/<namespace>/<secret-name>
     ```
  4. **Strict Disk Performance Requirements:** Because Raft requires synchronous disk flushes (`fsync`) to commit transactions to the Write-Ahead Log (WAL), `etcd` requires fast solid-state drives (NVMe or enterprise SSDs with fsync latency under 10ms). Slow mechanical hard drives or throttled cloud block storage cause leader election timeouts and cluster-wide API freezes.

### 3.3 `kube-scheduler`: The Intelligent Workload Placement Engine
- **What it is:** The control plane process that monitors newly created Pods that have no assigned node (`spec.nodeName == ""`) and selects the optimal worker node to run them.
- **Why it is needed:** In a cluster with thousands of nodes possessing diverse CPU counts, memory capacities, GPU accelerators, network topologies, and security constraints, placing workloads manually is impossible. The scheduler automates bin-packing and resource optimization.
- **Working Mechanism (The Two-Phase Scheduling Algorithm):**

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE TWO-PHASE SCHEDULING ALGORITHM                   │
└────────────────────────────────────────────────────────────────────────┘

 Unscheduled Pod ──► [ Phase 1: Filtering / Predicates ]
                      Filters out ineligible nodes:
                      - NodeResourcesFit (Enough CPU and RAM?)
                      - NodeName (Specific host requested?)
                      - NodePorts (Host network port conflicts?)
                      - NodeAffinity (RequiredDuringScheduling labels?)
                      - Taints and Tolerations (Can pod tolerate taints?)
                            │
                            ▼
                      [ Phase 2: Scoring / Priorities ]
                      Ranks surviving eligible nodes (0 to 100):
                      - NodeResourcesLeastAllocated (Balances cluster load)
                      - ImageLocalityPriority (Is container image already cached?)
                      - PodTopologySpread (Spreads across Availability Zones)
                      - NodeAffinityPriority (Preferred labels match)
                            │
                            ▼
                      Node with HIGHEST total score selected!
                            │
                            ▼
                      [ Phase 3: Binding ]
                      Writes binding back to kube-apiserver:
                      spec.nodeName = "worker-node-03"
```

### 3.4 `kube-controller-manager`: The Continuous Reconciliation Engine
- **What it is:** A single daemon binary that bundles dozens of independent, asynchronous control loops into a unified process.
- **Why it is needed:** Kubernetes is built on a **declarative model** rather than an imperative model. You do not tell Kubernetes *how to build infrastructure*; you declare the *desired end-state* (e.g., "I want 5 replicas of the API server running"). The controller manager continuously drives the current state toward that desired state.
- **Working Mechanism (The Reconciliation Control Loop):**

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE RECONCILIATION CONTROL LOOP                      │
└────────────────────────────────────────────────────────────────────────┘

                         ┌─────────────────────┐
                         │   1. OBSERVE        │
                         │   Query API Server  │
                         │   for Current State │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   2. ANALYZE        │
                         │   Compare Desired   │
                         │   vs. Current State │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   3. ACT            │
                         │   Issue CRUD calls  │
                         │   to reconcile diff │
                         └──────────┬──────────┘
                                    │
                                    └─────────────────── Loop repeats!
```

- **Core Controllers Bundled Inside the Binary:**
  1. **Node Lifecycle Controller:** Monitors kubelet node heartbeats. If a node stops reporting status for 40 seconds, it marks the node as `NotReady`. If it remains unreachable for 5 minutes (`pod-eviction-timeout`), the controller evicts all pods and reschedules them on healthy nodes.
  2. **Deployment & ReplicaSet Controller:** Ensures the exact number of pod replicas declared in the Deployment manifest exist in real time. If a pod crashes or a worker node catches fire, it instantly creates a replacement pod.
  3. **EndpointSlice Controller:** Watches Services and matching Pods, updating low-latency `EndpointSlice` resources so network traffic routes exclusively to healthy, running pods.
  4. **Job & CronJob Controller:** Spawns batch pods to completion and triggers scheduled cron tasks according to time-based schedules.
  5. **Garbage Collector Controller:** Reaps deleted resources, cleans up orphaned pods, and enforces cascading deletion based on `ownerReferences`.

---

## 💥 4. Failure Mode Forensics: What Happens When Components Die?

Understanding component failure modes is vital for architecting High Availability (HA) clusters and troubleshooting production outages:

| Component Outage | Impact on Running Pods | Impact on Cluster Administration | Recovery Procedure |
| :--- | :--- | :--- | :--- |
| **`kube-apiserver` Fails** | **Zero Impact.** Active pods and services continue executing and passing network traffic uninterrupted! | **Total Admin Freeze.** `kubectl`, CI/CD pipelines, and auto-scalers cannot read or mutate cluster state. | Restart API server container or fail over via HA load balancer. |
| **`etcd` Loses Quorum** | **Zero Impact.** Active worker nodes continue running existing pods using local kubelet caches. | **Read-Only / Dead State.** API server rejects all read and write mutations until quorum is restored. | Restore etcd snapshot from backup (`etcdctl snapshot restore`) or boot missing nodes. |
| **`kube-scheduler` Fails** | **Zero Impact.** Existing pods continue running normally. | **Pods Stuck in `Pending`.** New pods are created in etcd but remain unassigned (`spec.nodeName == ""`). | Standby scheduler in HA leader election takes over automatically. |
| **`kube-controller-manager` Fails** | **Zero Impact.** Existing pods continue running normally. | **No Self-Healing.** Crashed pods are NOT replaced. Deployments cannot scale up or down. | Standby controller manager in HA leader election takes over automatically. |

---

## 🛠️ 5. Practical Control Plane Diagnostics & Verification Commands

```bash
# 1. View high-level health of control plane components
kubectl get componentstatuses

# 2. View running control plane static pods (in kubeadm-provisioned clusters)
kubectl get pods -n kube-system -l tier=control-plane -o wide

# 3. Check etcd cluster health and member status via etcdctl
sudo ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  endpoint health

# 4. View real-time scheduler placement decisions and binding events
kubectl get events -n default --field-selector reason=Scheduled

# 5. Inspect API server audit logs on control plane host
tail -f /var/log/kubernetes/audit/audit.log
```

---

## 📋 6. Summary and Navigation

The Kubernetes Control Plane separates concerns through specialized, decoupled components: the stateless `kube-apiserver` provides a secure declarative gateway, `etcd` maintains strong Raft consistency, `kube-scheduler` optimizes two-phase placement, and `kube-controller-manager` drives infinite reconciliation loops. Together, they create an autonomous, self-healing operating system for distributed cloud computing.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (07 - Containers & Docker)](../../07%20-%20Containers%20%26%20Docker/11-Container-Observability-and-Troubleshooting/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Node Components Kubelet KubeProxy and CRI →](./02-Node-Components-Kubelet-KubeProxy-and-CRI.md) |
