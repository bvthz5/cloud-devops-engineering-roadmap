# 06 - Overlay Networks and Multi-Host VXLAN

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to multi-host container networking, Virtual Extensible LAN (VXLAN) encapsulation on UDP port 4789, gossip protocol control planes, IPsec data plane encryption, and firewall port requirements.

---

## 🏛️ 1. What Is an Overlay Network & Why Is It Essential for Clusters?

When software systems outgrow a single physical host and transition to a distributed multi-node cluster (such as Docker Swarm or Kubernetes), standalone virtual bridge networks are no longer sufficient.

A standard `bridge` network is strictly confined to the single Linux kernel where it was instantiated. If an API container on **Worker Node 1** needs to communicate with a database container running on **Worker Node 2**, an operator using simple bridge networking would have to:
1. Publish the database's port to Worker Node 2's public or private host IP.
2. Configure the API on Worker Node 1 to connect to that host IP and port.
3. Manage external firewalls, certificates, and IP updates as containers reschedule across nodes.

An **Overlay Network** solves this complexity by establishing a software-defined, flat Layer-2 virtual network that spans across multiple physical or cloud virtual machines. Containers attached to an overlay network communicate directly with one another as if they were plugged into the same physical local switch, completely abstracting away the physical host nodes beneath them.

---

## ⚙️ 2. How VXLAN (Virtual Extensible LAN) Encapsulation Works

Docker implements overlay networking using the standardized **VXLAN (Virtual Extensible LAN)** protocol (defined in IETF RFC 7348). VXLAN encapsulates Layer-2 Ethernet frames within standard Layer-4 UDP datagrams:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   VXLAN PACKET ENCAPSULATION LIFECYCLE                 │
└────────────────────────────────────────────────────────────────────────┘

 [ Worker Node 1 (Host IP: 192.168.1.10) ]
   Container A (IP: 10.0.0.4) sends packet to Container B (IP: 10.0.0.5)
   │
   ▼
 [ VXLAN Tunnel Endpoint (VTEP) on Node 1 ]
   - Intercepts raw Ethernet frame from Container A.
   - Wraps frame inside a standard UDP packet!
   - Outer Header: Source 192.168.1.10:4789 ──► Dest 192.168.1.20:4789
   - Adds VXLAN Network Identifier (VNI) Header.
   │
   ▼
 [ Physical Underlay Network (Switches / Routers / Internet) ]
   Routable standard UDP packets travel across normal network infrastructure.
   │
   ▼
 [ VXLAN Tunnel Endpoint (VTEP) on Node 2 ]
   - Receives UDP packet on port 4789.
   - Strips off outer UDP and VXLAN headers (Decapsulation).
   - Recovers the original inner Ethernet frame!
   │
   ▼
 [ Worker Node 2 (Host IP: 192.168.1.20) ]
   Container B (IP: 10.0.0.5) receives packet seamlessly!
```

### Key Components of the Architecture:
1. **The Underlay Network:** The physical network hardware, routers, switches, and host operating systems that provide IP connectivity between the cluster nodes.
2. **The Overlay Network:** The virtual software-defined network (e.g. `10.0.0.0/24`) that exists purely in software and connects container endpoints.
3. **VXLAN Tunnel Endpoint (VTEP):** A kernel-level network interface instantiated on each Docker host that performs the encapsulation and decapsulation of frames.
4. **VXLAN Network Identifier (VNI):** A 24-bit identifier in the VXLAN header that allows up to **16 million isolated virtual subnets** to coexist on the exact same physical infrastructure without traffic leakage.

---

## 🔬 3. The Control Plane: Gossip Protocol vs. Broadcast Flooding

In traditional physical Ethernet networks, when a switch does not know which physical port owns an IP address, it floods an **Address Resolution Protocol (ARP) broadcast packet** across every port on the network.

If an overlay network used broadcast flooding across cloud virtual machines, the cloud network would quickly suffer from catastrophic broadcast storms.

### The Swarm Distributed Control Plane:
Docker Swarm avoids broadcast storms by running an in-memory **Gossip Protocol (based on HashiCorp Serf)** over **TCP/UDP port 7946**:
- Whenever a container is scheduled, launched, or moved to a new node, the local Docker daemon announces the container's IP, MAC address, and host location to all other nodes via the gossip protocol.
- Each node maintains a local, distributed lookup table.
- When Container A sends a packet to Container B, the VTEP queries its local lookup table and instantly identifies the exact target host IP, transmitting the UDP packet directly via unicast without any ARP broadcast.

---

## 🔒 4. Securing Cross-Host Traffic with IPsec Data Plane Encryption

When container traffic traverses cloud networks (such as Amazon Web Services, Google Cloud Platform, or hybrid cloud links between on-premise datacenters and the cloud), plaintext container packets can be intercepted by network sniffers.

Docker provides native, kernel-level **IPsec Encryption** for overlay networks:

```bash
# Provision a cryptographically encrypted multi-host overlay network
docker network create \
  --driver overlay \
  --opt encrypted \
  --subnet 10.10.0.0/16 \
  production_secure_mesh
```

### How Overlay Encryption Operates:
When `--opt encrypted` is configured, Docker's `libnetwork` configures the Linux kernel's **IPsec Encapsulating Security Payload (ESP)** subsystem using hardware-accelerated **AES-GCM encryption** (IP protocol 50). All VXLAN UDP packets are encrypted at the kernel level before leaving the host network interface and decrypted upon arrival at the destination node.

---

## 🛡️ 5. Mandatory Firewall Port Configuration for Overlay Clusters

For Docker Swarm and overlay networking to operate across firewalled virtual machines, the following host ports must be explicitly opened:

| Port / Protocol | Direction | Purpose & Architectural Subsystem |
| :--- | :---: | :--- |
| **TCP Port 2377** | Inbound | **Cluster Management:** Used by Swarm manager nodes for Raft consensus and state sync. |
| **TCP & UDP Port 7946** | Inbound / Outbound | **Node Communication:** Used by the Gossip control plane for service discovery and node health. |
| **UDP Port 4789** | Inbound / Outbound | **Overlay Data Plane:** The standard IETF port used for all VXLAN packet traffic. |
| **IP Protocol 50 (ESP)** | Inbound / Outbound | **IPsec Encryption:** Must be permitted when creating encrypted (`--opt encrypted`) overlay networks. |

---

## 📋 6. Summary and Navigation

Overlay networks represent the networking backbone of multi-host container clustering. By leveraging VXLAN encapsulation on UDP port 4789, gossip-based control planes, and native IPsec encryption, DevOps engineers build secure, seamless, and high-performance container fabrics that effortlessly scale across distributed enterprise infrastructure.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Host None and Macvlan Networking](./05-Host-None-and-Macvlan-Networking.md) | [Index](../../../README.md) | [07 - Real-World Scenarios →](./07-Real-World-Scenarios.md) |
