# 01 - Kubernetes Networking Model and IP-per-Pod Rule

## 1. Why Docker NAT Failed at Scale: The Genesis of the Kubernetes Networking Model

In early container ecosystems (specifically standard Docker Engine), containers relied on **Host Port Mapping and Network Address Translation (NAT)**. When running a containerized service listening on port 80, the operator was forced to bind it to an arbitrary host port using `-p 8080:80` or `-p 31234:80`.

In large-scale production clusters running thousands of containers, this port-mapping model caused immediate operational failure:
1. **Port Allocation Collisions:** Two distinct teams running web microservices on port 80 could not be scheduled onto the same physical machine unless an orchestration layer dynamically negotiated random, non-conflicting host ports (e.g., ports 32112 and 32113).
2. **Configuration Complexity:** Microservices could not know their own reachable endpoints in advance. An application had to query host metadata APIs to discover which random external port was mapped to its internal port 80.
3. **Firewall and Observability Breakdown:** NAT obscured client source IP addresses, rendering network packet analyzers, IDS/IPS firewalls, and audit logs ineffective.

Kubernetes permanently eliminated port translation by introducing the **IP-per-Pod networking model**. In Kubernetes, every Pod is treated like a real, standalone physical server or virtual machine connected directly to a high-speed corporate network.

```text
+---------------------------------------------------------------------------------------------------+
|                        DOCKER HOST PORT MAPPING VS KUBERNETES IP-PER-POD                          |
+---------------------------------------------------------------------------------------------------+

 DOCKER ENGINE MODEL (PORT TRANSLATION HELL):
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Host VM (192.168.1.50)                                                           │
 │   ├── Container A (Port 80)  ──► Mapped to Host Port: 8080                       │
 │   └── Container B (Port 80)  ──► Port Collision! Must map to random Port: 31245  │
 └──────────────────────────────────────────────────────────────────────────────────┘
 Result: External clients must track random ports; containers do not know their own public ports!

 KUBERNETES MODEL (CLEAN IP-PER-POD):
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Host VM (192.168.1.50)                                                           │
 │   ├── Pod A (IP: 10.244.1.15) ──► Listens cleanly on Port 80                     │
 │   └── Pod B (IP: 10.244.1.16) ──► Listens cleanly on Port 80                     │
 └──────────────────────────────────────────────────────────────────────────────────┘
 Result: Zero port collisions! Pod A and Pod B both listen on standard port 80 on separate IPs.
```

---

## 2. The 3 Fundamental Tenets of Kubernetes Networking

The official Kubernetes networking specification enforces three strict, non-negotiable architectural invariants that every cluster implementation (CNI plugin) must satisfy:

1. **Every Pod can communicate with every other Pod across all nodes without Network Address Translation (NAT):** When Pod A (`10.244.1.5`) transmits a TCP packet to Pod B (`10.244.2.8`), the packet arrives at Pod B with the source IP header intact (`10.244.1.5`). The host operating system does not perform SNAT (Source NAT) or IP masquerading.
2. **All Nodes can communicate with all Pods (and vice versa) without NAT:** Any host agent running on a worker node (such as the Kubelet or monitoring daemons) can open direct socket connections to any Pod IP address without port mapping.
3. **The IP that a Pod sees as its own IP is the exact same IP that all other Pods see for it:** Self-consistency is guaranteed. If an application inside a container executes `hostname -I` or inspects `eth0`, the IP address returned matches the exact IP address other services in the cluster use to dial it.

---

## 3. The 4 Distinct Communication Scenarios

A complete understanding of Kubernetes networking requires examining the four distinct data paths:

```text
+---------------------------------------------------------------------------------------------------+
|                           THE 4 KUBERNETES NETWORKING DATA PATHS                                  |
+---------------------------------------------------------------------------------------------------+

 1. Container-to-Container (Inside Same Pod):
    Mechanism: Shared Linux Network Namespace (`netns`).
    Data Path: Communicates over the Linux loopback interface (`localhost` / `127.0.0.1`).
    Port Sharing: Containers in the same pod share the same port space; container A on :8080 can
                  talk to container B on :5432 directly.

 2. Pod-to-Pod (On the Same Physical Node):
    Mechanism: Linux Virtual Ethernet (`veth`) pair and local Linux bridge (`cbr0`) or eBPF datapath.
    Data Path: Pod A eth0 ──► vethA ──► Linux Bridge (cbr0) ──► vethB ──► Pod B eth0.
    Performance: Pure kernel memory copy; near-zero latency; packets never touch the physical NIC.

 3. Pod-to-Pod (Across Different Worker Nodes):
    Mechanism: Handled by CNI Plugin via Overlay (VXLAN/Geneve) or Flat Underlay Routing (BGP/VPC).
    Data Path:
      [Pod A: 10.244.1.5] ──► Host 1 Physical NIC (192.168.1.10)
                                      │ Physical Wire (LAN / Cloud VPC)
                                      ▼
      [Pod B: 10.244.2.8] ◄── Host 2 Physical NIC (192.168.1.11)

 4. Pod-to-External Internet (Egress Traffic):
    Mechanism: Linux Netfilter / iptables `MASQUERADE` (SNAT).
    Data Path: Pod private IP (`10.244.1.5`) is translated to the worker node's routable host IP
               (`192.168.1.10`) as the packet exits the node's physical interface (`eth0`).
```

---

## 4. Under the Hood: The Linux `veth` Pair Plumbing

How does the Linux kernel physically connect an isolated container namespace to the rest of the node?

When a Pod is scheduled to a node, the Container Network Interface (CNI) creates a **Virtual Ethernet (`veth`) pair**, which functions like a virtual bidirectional fiber optic cable:
- One end of the cable is placed inside the Pod's private network namespace and renamed `eth0`.
- The opposite end remains in the host's root network namespace (named something like `veth8f9a2b`) and is attached to a local Linux bridge (`cbr0`) or wired directly into an eBPF routing hook.

```text
+---------------------------------------------------------------------------------------------------+
|                        LINUX VIRTUAL ETHERNET (veth) PAIR PLUMBING                                |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ WORKER NODE (Root Network Namespace)                                                           │
 │                                                                                                 │
 │  ┌────────────────────────────────────────────────────────┐                                     │
 │  │ Linux Bridge (cbr0) / eBPF Datapath                    │                                     │
 │  └─────────────┬─────────────────────────────┬────────────┘                                     │
 │                │                             │                                                  │
 │       vethA (Host End)              vethB (Host End)                                            │
 │                │                             │                                                  │
 └────────────────┼─────────────────────────────┼──────────────────────────────────────────────────┘
                  │ (Virtual Wire)              │ (Virtual Wire)
 ┌────────────────┼──────────────┐ ┌────────────┼────────────────┐
 │ Pod A Network  │ Namespace    │ │ Pod B Network │ Namespace    │
 │                ▼              │ │               ▼              │
 │              eth0             │ │             eth0             │
 │         (10.244.1.5)          │ │         (10.244.1.6)         │
 └───────────────────────────────┘ └──────────────────────────────┘
```

---

## 5. Enterprise CIDR Allocation Architecture

In enterprise cluster planning, network architects must partition subnets into three non-overlapping CIDR blocks:

| CIDR Allocation Block | Example Subnet | Description & Scope |
| :--- | :--- | :--- |
| **Node Network CIDR** | `192.168.0.0/16` or `10.0.0.0/16` | Routable IP addresses assigned by the cloud VPC or physical datacenter DHCP to the physical worker node VMs. |
| **Cluster Pod CIDR** | `10.244.0.0/16` (65,536 total IPs) | The global pool of IP addresses reserved exclusively for Pods across the entire cluster. |
| **Node Pod Subnet** | `10.244.1.0/24` (254 IPs per node) | A dedicated slice of the Cluster Pod CIDR allocated by the Controller Manager to each individual worker node for local container IP assignment. |
| **Service (ClusterIP) CIDR**| `10.96.0.0/12` | Virtual IP addresses assigned to Kubernetes Services. These IPs **never appear on physical network interfaces**; they exist purely as `iptables` / `IPVS` packet-rewriting rules. |

---

## 6. Operational Network Inspection Commands

```bash
# Check the Pod CIDR assigned to each worker node
kubectl get nodes -o custom-columns=NAME:.metadata.name,POD_CIDR:.spec.podCIDR

# Inspect a running pod's network interface directly
kubectl exec -it <pod-name> -- ip addr show eth0

# Verify the veth interface linked to a container from the worker node host
ip link show type veth

# Trace network route from inside a pod to an external destination
kubectl exec -it <pod-name> -- traceroute 8.8.8.8
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (13-Helm-Package-Manager-and-Charts)](../13-Helm-Package-Manager-and-Charts/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Container Network Interface CNI Specification →](./02-Container-Network-Interface-CNI-Specification.md) |
