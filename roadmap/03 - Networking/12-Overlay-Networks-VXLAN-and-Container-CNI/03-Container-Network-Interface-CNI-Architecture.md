# 03 - Container Network Interface (CNI) Architecture

## 1. How CNI Works in Kubernetes

The CNI specification is a CNCF standard defining how container runtimes (containerd, CRI-O) configure network interfaces for containers.

```
Kubelet initiates Pod creation
   │
   ▼
Container Runtime (containerd) creates Linux Network Namespace (`netns`)
   │
   ▼
Runtime executes CNI plugin binary (`/opt/cni/bin/calico` or `/opt/cni/bin/cilium`)
Passes JSON config via STDIN with Action: "ADD"
   │
   ▼
CNI Plugin:
 1. Creates a virtual ethernet pair (`veth-host` <---> `veth-pod`)
 2. Moves `veth-pod` inside the Pod's network namespace (renamed to `eth0`)
 3. Allocates an IP from IPAM (IP Address Management) pool
 4. Configures routes inside the Pod namespace pointing default gateway to `veth-host`
 5. Returns JSON response with allocated IP to container runtime
```

---

## 2. Linux Network Namespaces (`netns`) and `veth` Pairs

A **Network Namespace** provides an isolated instance of the network stack (its own routing table, firewall rules, and interfaces).
A **`veth` (Virtual Ethernet) pair** acts as a virtual network cable: packets entering one end automatically emerge from the other.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - VXLAN & Geneve](./02-VXLAN-and-Geneve-Protocol-Deep-Dive.md) | [README](./README.md) | [04 - Major CNIs Compared](./04-Comparing-Major-CNIs-Flannel-Calico-Cilium-AWS-VPC-CNI.md) |
