# 02 - Node Components: Kubelet, Kube-Proxy, and CRI

## 1. Worker Node Mechanics

Every worker node in a Kubernetes cluster executes three primary node-level components:
1. **`kubelet`**: The primary node agent.
2. **`kube-proxy`**: The network packet router and service abstraction engine.
3. **Container Runtime (CRI)**: The low-level and high-level runtime (e.g., `containerd`, `CRI-O`).

```text
+-------------------------------------------------------------------------------+
|                                WORKER NODE                                    |
|                                                                               |
|   +-----------------------------------------------------------------------+   |
|   |                              kubelet                                  |   |
|   | - Syncs PodSpecs from API Server                                      |   |
|   | - Reports Node Status, Capacity & Heartbeats                          |   |
|   | - Executes Liveness & Readiness Probes                                |   |
|   +-------------------+-------------------------------+-------------------+   |
|                       |                               |                       |
|          gRPC (/run/containerd/containerd.sock)       | (cgroups v2)          |
|                       v                               v                       |
|   +---------------------------------------+   +---------------------------+   |
|   |      CRI Runtime (containerd)         |   |     Linux Systemd         |   |
|   |  - Pulls images                       |   |     cgroup Manager        |   |
|   |  - Calls CNI for IP allocation        |   +---------------------------+   |
|   |  - Invokes runc/crun to spawn tasks   |                                   |
|   +---------------------------------------+                                   |
|                                                                               |
|   +-----------------------------------------------------------------------+   |
|   |                            kube-proxy                                 |   |
|   | - Watches Services & EndpointSlices                                   |   |
|   | - Programs iptables NAT rules or IPVS virtual servers                 |   |
|   +-----------------------------------------------------------------------+   |
+-------------------------------------------------------------------------------+
```

---

## 2. Kubelet Responsibilities & Flags

### 2.1 The Sync Loop (PLEG)
The `kubelet` uses the **Pod Lifecycle Event Generator (PLEG)** to inspect container runtimes periodically. If PLEG becomes unhealthy or latency exceeds threshold, the node transitions to `NotReady`.

### 2.2 Critical Kubelet Configuration (`/var/lib/kubelet/config.yaml`)
```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd             # MUST match containerd cgroup driver
containerLogMaxSize: 50Mi
containerLogMaxFiles: 5
evictionHard:
  memory.available: "200Mi"
  nodefs.available: "10%"
  imagefs.available: "15%"
systemReserved:
  cpu: "500m"
  memory: "1Gi"
kubeReserved:
  cpu: "500m"
  memory: "1Gi"
```

---

## 3. Kube-Proxy Datapaths: iptables vs IPVS

| Feature | iptables Mode | IPVS Mode (IP Virtual Server) |
|---|---|---|
| **Mechanism** | Sequential Netfilter chain evaluation | In-kernel Hash Tables |
| **Complexity** | $O(N)$ lookup where $N$ = total services | $O(1)$ constant time lookup |
| **Scale Limit** | Bottlenecks at ~5,000+ Services | Easily scales to 50,000+ Services |
| **Balancing Algorithms** | Random probability distribution | Round-Robin, Least-Connection, Source Hash |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Control Plane Components API etcd Scheduler ControllerManager](./01-Control-Plane-Components-API-etcd-Scheduler-ControllerManager.md) | [Index](../../../README.md) | [03 - Etcd Distributed Storage Quorum and Raft →](./03-Etcd-Distributed-Storage-Quorum-and-Raft.md) |
