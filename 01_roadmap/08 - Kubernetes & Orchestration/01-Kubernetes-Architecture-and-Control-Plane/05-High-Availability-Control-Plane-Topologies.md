# 05 - High-Availability Control Plane Topologies

## 1. Stacked vs External etcd Topologies

In production enterprise deployments, the control plane must survive the loss of master nodes without dropping user workloads or API access.

### Topology Comparison

```text
STACKED ETCD TOPOLOGY (Simpler, Standard Kubeadm)
+-----------------------+  +-----------------------+  +-----------------------+
| Master Node 01        |  | Master Node 02        |  | Master Node 03        |
| [kube-apiserver]      |  | [kube-apiserver]      |  | [kube-apiserver]      |
| [etcd member 1]       |  | [etcd member 2]       |  | [etcd member 3]       |
+-----------------------+  +-----------------------+  +-----------------------+
(etcd co-located on control plane nodes; less hardware, coupled failure domain)

EXTERNAL ETCD TOPOLOGY (Enterprise Isolation)
+-------------------+  +-------------------+  +-------------------+
| CP Node 01        |  | CP Node 02        |  | CP Node 03        |
| [kube-apiserver]  |  | [kube-apiserver]  |  | [kube-apiserver]  |
+---------+---------+  +---------+---------+  +---------+---------+
          |                      |                      |
          +----------------------+----------------------+
                                 | (Dedicated low-latency network)
          +----------------------+----------------------+
          |                      |                      |
+---------v---------+  +---------v---------+  +---------v---------+
| Dedicated etcd 01 |  | Dedicated etcd 02 |  | Dedicated etcd 03 |
+-------------------+  +-------------------+  +-------------------+
(etcd on dedicated NVMe bare-metal/VM instances; decoupled failure domains)
```

---

## 2. Load Balancing the API Server

Because `kube-apiserver` is stateless, multiple instances are fronted by a high-availability Layer 4 proxy:
- **Cloud Environments:** AWS Network Load Balancer (NLB), Azure Load Balancer, GCP Passthrough NLB.
- **Bare Metal / On-Premise:** HAProxy + Keepalived with Virtual IP (VIP), or kube-vip running as a static pod.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Kubernetes API Request Flow Authentication Admission](./04-Kubernetes-API-Request-Flow-Authentication-Admission.md) | [Index](../../../README.md) | [06 - Cloud Controller Manager and Provider Integrations →](./06-Cloud-Controller-Manager-and-Provider-Integrations.md) |
