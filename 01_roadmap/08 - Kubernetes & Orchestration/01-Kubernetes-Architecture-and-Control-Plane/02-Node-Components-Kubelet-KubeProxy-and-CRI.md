# 02 - Node Components: Kubelet, Kube-Proxy, and CRI

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Kubernetes worker node components, detailing `kubelet` sync loops, PLEG mechanics, node lease heartbeats, `kube-proxy` iptables vs. IPVS packet routing, and the Container Runtime Interface (CRI).

---

## 🏛️ 1. What Is a Worker Node & Why Are Node Components Decoupled?

In a Kubernetes cluster, while the control plane makes global decisions and stores desired state, the physical or virtual computing machines that execute container workloads are called **Worker Nodes**.

To maintain reliability, scalability, and security, Kubernetes strictly decouples workload execution from control plane governance. Worker nodes possess no direct access to `etcd` and do not participate in scheduling decisions. Instead, each worker node executes three specialized, decoupled components:
1. **`kubelet`:** The primary node management agent that ensures containers described in PodSpecs are running and healthy.
2. **`kube-proxy`:** The network routing engine that maintains network rules and provides virtual IP (VIP) load balancing for Kubernetes Services.
3. **Container Runtime (CRI):** The software responsible for pulling images, allocating namespaces, and executing container binaries (e.g., `containerd`, `CRI-O`).

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        KUBERNETES WORKER NODE                          │
└────────────────────────────────────────────────────────────────────────┘

 ┌──────────────────────────────────────────────────────────────────────┐
 │                              kubelet                                 │
 │  - Syncs PodSpecs from kube-apiserver via secure TLS                 │
 │  - Runs Pod Lifecycle Event Generator (PLEG) loop                    │
 │  - Renews Node Lease heartbeats (every 10s)                          │
 │  - Executes Liveness, Readiness, and Startup health probes          │
 │  - Enforces resource limits and node eviction boundaries             │
 └──────────────────┬───────────────────────────────┬───────────────────┘
                    │                               │
                    │ (gRPC via Unix Socket)        │ (cgroups v2 Systemd)
                    ▼                               ▼
 ┌──────────────────────────────────────┐  ┌────────────────────────────┐
 │    Container Runtime Interface (CRI) │  │       Linux systemd        │
 │    (containerd / CRI-O)              │  │       cgroup Driver        │
 │  - Pulls OCI image layers            │  └────────────────────────────┘
 │  - Invokes CNI for IP allocation     │
 │  - Executes low-level runc / crun    │
 └──────────────────────────────────────┘
                    │
                    ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │                             kube-proxy                               │
 │  - Watches Services and EndpointSlices from API server               │
 │  - Programs Linux kernel iptables NAT chains or IPVS virtual servers │
 │  - Translates Virtual ClusterIPs to active backend Pod IPs           │
 └──────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 2. Deep Dive: `kubelet` (The Node Captain)

The **`kubelet`** is the primary daemon agent running on every node in the cluster. It does not run as a container; it runs as a native `systemd` service directly on the host operating system.

### 2.1 How the Kubelet Works Internally
The kubelet operates on the concept of a **PodSpec**—a declarative YAML or JSON specification describing the containers, environment variables, mounts, and network configurations of a Pod.
1. **Watching the API Server:** The kubelet maintains a persistent HTTPS connection to the `kube-apiserver`. Whenever the `kube-scheduler` binds a pod to its specific node (`spec.nodeName == nodeName`), the API server notifies the kubelet.
2. **Pod Lifecycle Event Generator (PLEG):**
   - The kubelet uses **PLEG** to continuously synchronize the state of local containers.
   - Every second, PLEG issues a gRPC call to the container runtime to inspect running processes. It compares the runtime state with its internal cache and generates discrete lifecycle events (e.g., `ContainerStarted`, `ContainerDied`).
   - If the container runtime hangs or storage I/O becomes unresponsive, PLEG health checks fail (`PLEG is not healthy`), causing the node status to transition to **`NotReady`**.
3. **Node Heartbeats via Node Leases:**
   - In early Kubernetes versions, nodes updated their complete `Node` object in etcd every 10 seconds. In large clusters with 5,000 nodes, this saturated etcd with millions of write transactions.
   - In modern Kubernetes, the kubelet sends lightweight heartbeats using **Node Leases** (`coordination.k8s.io` API group). A lease is a tiny object renewed every **10 seconds**. As long as the lease updates, the control plane knows the node is online with minimal etcd overhead.

### 2.2 Static Pods: The Secret to Bootstrapping Kubernetes
A unique capability of the kubelet is running **Static Pods**. A static pod is a pod managed directly by the kubelet without the API server or scheduler being involved.
- The kubelet watches a local directory on disk: `/etc/kubernetes/manifests/`.
- Any YAML file placed in this directory is automatically spawned by the kubelet.
- **Why this is critical:** In clusters bootstrapped by `kubeadm`, the control plane components (`kube-apiserver`, `etcd`, `kube-scheduler`, `kube-controller-manager`) run as **Static Pods**! The local kubelet boots the API server before the cluster even exists.

### 2.3 Resource Reservations & Eviction Hard Boundaries
To prevent user workloads from consuming all physical RAM and crashing the host OS or kubelet, operators configure resource reservations in `/var/lib/kubelet/config.yaml`:

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd             # Mandatory: Must match containerd cgroup driver!

# System & K8s Reservations (Subtracted from Allocatable Capacity)
systemReserved:
  cpu: "500m"
  memory: "1Gi"
kubeReserved:
  cpu: "500m"
  memory: "1Gi"

# Hard Eviction Boundaries (Kubelet evicts pods if breached)
evictionHard:
  memory.available: "200Mi"       # Evicts lowest priority pods if RAM < 200Mi
  nodefs.available: "10%"         # Evicts pods if root disk space < 10%
  imagefs.available: "15%"        # Evicts pods if container image disk < 15%
```

---

## 🌐 3. Deep Dive: `kube-proxy` (The Network Routing Engine)

In Kubernetes, Pods are ephemeral and constantly receive new IP addresses. To provide a stable network address, Kubernetes creates **Services** with a static **ClusterIP (Virtual IP)**.

However, a ClusterIP is a purely **virtual IP address**—there is no physical network card or virtual interface on the host with that IP. **`kube-proxy`** is the component that makes virtual IPs work.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   KUBE-PROXY PACKET ROUTING PIPELINE                   │
└────────────────────────────────────────────────────────────────────────┘

 Client Pod makes request to Service Virtual IP: 10.96.0.10:80
                           │
                           ▼
 Linux Netfilter Kernel Layer (Configured by kube-proxy)
 Intercepts destination IP 10.96.0.10:80 before packet leaves host!
                           │
         ┌─────────────────┴─────────────────┐
         ▼                                   ▼
 [ Mode 1: iptables ]                [ Mode 2: IPVS ]
 Evaluates sequential chains         Consults in-kernel hash table
 Picks backend Pod via probability   Picks backend via weighted round-robin
                           │
                           ▼
 Rewrites Destination IP to active Backend Pod IP: 10.244.1.45:8080
 Packet travels across CNI network directly to target container!
```

### 3.1 Comparison of `kube-proxy` Operating Modes

| Architectural Dimension | `iptables` Mode (Default) | `IPVS` Mode (High Scale) | eBPF Replacement (Cilium) |
| :--- | :--- | :--- | :--- |
| **Underlying Technology** | Linux Netfilter iptables chains | Linux IP Virtual Server (LVS) | Linux Extended Berkeley Packet Filter |
| **Lookup Time Complexity** | **$O(N)$ Sequential Evaluation** | **$O(1)$ Hash Table Lookup** | **$O(1)$ BPF Map Lookup** |
| **Performance at 5,000+ Services** | Severe packet latency degradation; CPU spikes during rule sync | High throughput; negligible CPU overhead | Bare-metal performance; skips Netfilter entirely |
| **Load Balancing Algorithms** | Random probability distribution only | Round-Robin, Least-Connection, Source-Hash | Advanced weighted, latency-aware routing |
| **Production Scale Limit** | Small to Medium clusters (< 1,000 Services) | Enterprise High Scale (10,000+ Services) | Next-generation cloud-native scale |

---

## 📦 4. Deep Dive: Container Runtime Interface (CRI)

Historically, Kubernetes was hardcoded to invoke the Docker daemon (`dockerd`). Because Docker was designed for human developers rather than automated orchestrators, it introduced unnecessary overhead and dependencies.

In Kubernetes v1.5, the community introduced the **Container Runtime Interface (CRI)**—a standardized gRPC API specification that decouples Kubernetes from specific container runtimes.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CRI LAYERED RUNTIME ARCHITECTURE                     │
└────────────────────────────────────────────────────────────────────────┘

 [ kubelet ]
      │ (CRI gRPC calls: RunPodSandbox, CreateContainer, StartContainer)
      ▼
 [ High-Level Runtime: containerd / CRI-O ]
   - Manages complete container lifecycle
   - Pulls OCI image layers from registries
   - Invokes CNI plugins to allocate IP addresses and setup veth pairs
   - Unpacks rootfs layers onto host disk
      │
      │ (OCI Runtime Specification: config.json + rootfs)
      ▼
 [ Low-Level Runtime: runc / crun ]
   - Configures Linux kernel namespaces (clone/unshare)
   - Configures cgroups v2 limits
   - Executes pivot_root to jail container
   - Hands off execution to application binary
```

### 4.1 The Dockershim Deprecation
In Kubernetes v1.20, the legacy translation layer **`dockershim`** was officially deprecated, and in Kubernetes v1.24, it was completely removed. Modern clusters interface directly with dedicated, lightweight CRI runtimes (**`containerd`** or **`CRI-O`**), eliminating redundant daemon hops and slashing memory footprint.

---

## 🛠️ 5. Practical Node Diagnostics & Verification Commands

```bash
# 1. Inspect worker node health, conditions, and capacity
kubectl describe node worker-node-01

# 2. Check if kubelet service is active on host
sudo systemctl status kubelet

# 3. Stream real-time kubelet logs to diagnose startup or probe failures
sudo journalctl -u kubelet -f -n 100

# 4. Use crictl to inspect CRI containers directly (bypassing Kubernetes API)
sudo crictl pods
sudo crictl ps
sudo crictl logs <container-id>

# 5. Verify active kube-proxy mode (iptables vs IPVS)
kubectl logs -n kube-system -l k8s-app=kube-proxy | grep "Using"
```

---

## 📋 6. Summary and Navigation

Worker nodes provide the compute, memory, and networking muscle of Kubernetes. By combining the `kubelet` for lifecycle governance, `kube-proxy` for virtual IP service routing, and CRI-compliant runtimes like `containerd` for lightweight execution, nodes reliably host mission-critical containerized applications.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Control Plane Components API etcd Scheduler ControllerManager](./01-Control-Plane-Components-API-etcd-Scheduler-ControllerManager.md) | [Index](../../../README.md) | [03 - Etcd Distributed Storage Quorum and Raft →](./03-Etcd-Distributed-Storage-Quorum-and-Raft.md) |
