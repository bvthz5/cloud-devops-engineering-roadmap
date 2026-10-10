# 01 - Control Plane Components: API, etcd, Scheduler, and Controller Manager

## 1. History of Kubernetes & Container Orchestration

| Technology / Milestone | Inventor(s) & Organization | Year | The Real Problem / Crisis That Triggered It | Real-World Impact |
| :--- | :--- | :--- | :--- | :--- |
| **Google Borg** | **Google Systems Infrastructure Team** (Urs Hölzle, Abhishek Verma et al.) | **2003–2004** | Google operated hundreds of thousands of bare-metal machines running Search and Gmail. When hardware servers died, engineers could not manually SSH to reboot tasks. Borg automated bin-packing, dynamic scheduling, and self-healing. | Proved that containerized orchestrators could manage global web-scale infrastructure autonomously. |
| **Google Omega** | **Google Cluster Management Team** (Brian Grant, Malte Schwarzkopf et al.) | **2013** | Borg had a monolithic centralized scheduler that became a lock contention bottleneck as clusters expanded. Omega introduced a shared-state transaction database architecture. | Formed the direct architectural predecessor to Kubernetes' declarative API Server + `etcd` model. |
| **The Kubernetes Project ("Project Seven")** | **Joe Beda, Brendan Burns, Craig McLuckie** (Google, with Tim Hockin, Brian Grant) | **June 2014 (v1.0 July 2015)** | With Docker exploding in 2013, developers had isolated containers but no standard open way to run 10,000 containers across 50 servers with zero downtime. Google wrote Kubernetes in Go based on 10+ years of Borg/Omega experience. | Became the de-facto operating system of the cloud and the foundation of modern cloud-native computing. |
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

## 2. High-Level Control Plane Architecture

The Kubernetes Control Plane makes global decisions about the cluster (e.g., scheduling workloads), detects and responds to cluster events, and maintains the desired state declared in your YAML manifests.

```text
+-----------------------------------------------------------------------------------+
|                            KUBERNETES CONTROL PLANE                               |
|                                                                                   |
|  +--------------------+         +-----------------------+                         |
|  |   kube-scheduler   |         | kube-controller-mgr   |                         |
|  +---------+----------+         +-----------+-----------+                         |
|            |                                |                                     |
|            +---------------+   +------------+                                     |
|                            |   |                                                  |
|                            v   v                                                  |
|                    +--------------------+         +----------------------------+  |
|   kubectl / CI ───►|  kube-apiserver    |◄───────►|            etcd            |  |
|                    +---------+----------+         |   (Distributed Key-Value)  |  |
|                              |                    +----------------------------+  |
|                              |                                                    |
+------------------------------|----------------------------------------------------+
                               | (Secure gRPC / HTTPS TLS)
                               v
+-----------------------------------------------------------------------------------+
|                              WORKER NODE POOL                                     |
|  +-----------------------------------+     +-----------------------------------+  |
|  | Node 01                           |     | Node 02                           |  |
|  | [kubelet] ──► [CRI / containerd]  |     | [kubelet] ──► [CRI / containerd]  |  |
|  | [kube-proxy] (iptables / IPVS)    |     | [kube-proxy] (iptables / IPVS)    |  |
|  +-----------------------------------+     +-----------------------------------+  |
+-----------------------------------------------------------------------------------+
```

---

## 3. Deep Component Breakdown

### 2.1 `kube-apiserver` (The Gateway)
- **Role:** The only component that talks directly to `etcd`. All other components (scheduler, controller manager, kubelet) communicate exclusively with the API server.
- **Stateless:** Can be horizontally scaled behind a Layer 4 (HAProxy, AWS NLB) or Layer 7 load balancer.
- **Port:** Default HTTPS on port `6443`.
- **Functions:**
  1. Serves the REST API.
  2. Authenticates and authorizes requests.
  3. Executes mutating and validating admission webhooks.
  4. Manages optimistic concurrency control via `metadata.resourceVersion`.

### 2.2 `etcd` (The Single Source of Truth)
- **Role:** Consistent and highly-available key-value store based on the Raft consensus algorithm.
- **Storage:** Stores cluster state under the `/registry` prefix (e.g., `/registry/pods/default/my-pod`).
- **Characteristics:** Requires low-latency NVMe/SSD storage (disk fsync < 10ms); otherwise leader election timeouts cascade into cluster failure.

### 2.3 `kube-scheduler` (The Placement Engine)
- **Role:** Watches for newly created Pods with no `spec.nodeName` assigned and selects an optimal worker node.
- **Two-Phase Algorithm:**
  1. **Filtering (Predicates):** Filters out nodes that cannot satisfy the pod requirements (e.g., insufficient CPU/RAM, node taints without tolerations, disk pressure, port conflicts).
  2. **Scoring (Priorities):** Ranks the remaining nodes to find the best match (e.g., node affinity preference, least requested resources, image locality).

### 2.4 `kube-controller-manager` (The Reconciliation Engine)
- **Role:** Runs core control loops continuously comparing **Current State** with **Desired State**.
- **Controllers Packed in Single Binary:**
  - `Node Lifecycle Controller`: Detects when nodes go offline and handles eviction.
  - `Deployment / ReplicaSet Controller`: Ensures the specified number of pod replicas are running.
  - `EndpointSlice Controller`: Populates endpoints for Services.
  - `ServiceAccount Controller`: Creates default ServiceAccounts and secrets for namespaces.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (07 - Containers & Docker)](../../07%20-%20Containers%20%26%20Docker/11-Container-Observability-and-Troubleshooting/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Node Components Kubelet KubeProxy and CRI →](./02-Node-Components-Kubelet-KubeProxy-and-CRI.md) |
