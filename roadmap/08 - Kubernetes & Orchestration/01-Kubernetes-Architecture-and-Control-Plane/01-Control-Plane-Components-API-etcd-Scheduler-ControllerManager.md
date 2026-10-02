# 01 - Control Plane Components: API, etcd, Scheduler, and Controller Manager

## 1. High-Level Control Plane Architecture

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

## 2. Deep Component Breakdown

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
| [README](./README.md) | [README](./README.md) | [02 - Node Components](./02-Node-Components-Kubelet-KubeProxy-and-CRI.md) |
