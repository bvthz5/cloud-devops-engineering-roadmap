# 01 - Docker Network Architecture and Drivers

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Container Network Model (CNM), the `libnetwork` subsystem, and the five built-in Docker network drivers (`bridge`, `host`, `none`, `overlay`, `macvlan`).

---

## 🏛️ 1. What Is Docker Networking & The Container Network Model (CNM)?

When containers execute on a server, they must communicate with external client browsers, interact with internal databases, and exchange messages with sibling microservices.

To manage container connectivity in a standardized, vendor-neutral manner, Docker designed the **Container Network Model (CNM)**. The CNM specification is implemented internally by Docker through an open-source Go library called **`libnetwork`**.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CONTAINER NETWORK MODEL (CNM)                        │
└────────────────────────────────────────────────────────────────────────┘

 ┌──────────────────────────────────────────────────────────────────────┐
 │ 1. Network Sandbox:                                                  │
 │    The isolated Linux network namespace of the container. Contains   │
 │    the container's routing tables, iptables rules, and network       │
 │    interfaces (e.g., eth0, lo).                                      │
 └──────────────────────────────────┬───────────────────────────────────┘
                                    │
                                    ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 2. Endpoint:                                                         │
 │    The virtual network interface (veth pair) that connects a         │
 │    Sandbox to a specific Network. An endpoint belongs to exactly     │
 │    one Sandbox and one Network.                                      │
 └──────────────────────────────────┬───────────────────────────────────┘
                                    │
                                    ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │ 3. Network:                                                          │
 │    A software-defined collection of endpoints that can communicate   │
 │    with each other directly (e.g., a virtual bridge switch or VXLAN).│
 └──────────────────────────────────────────────────────────────────────┘
```

The CNM decouples the high-level application definition from the underlying physical infrastructure, allowing microservices to be attached to multiple networks dynamically using pluggable network drivers.

---

## ⚙️ 2. The Five Built-In Docker Network Drivers

Docker provides five native network drivers, each designed for distinct operational architectures, security requirements, and performance envelopes:

### 2.1 The `bridge` Driver (Single-Host Virtual Switch)
- **What it is:** The default network driver for standalone containers.
- **Working Mechanism:** Creates a software-defined Linux bridge (a virtual Ethernet switch) on the host machine. Docker allocates a private private subnet (e.g., `172.17.0.0/16`), assigns each attached container an isolated IP address, and provisions virtual Ethernet cable pairs (`veth`) connecting the container's network namespace to the bridge.
- **Traffic Routing:** Outbound traffic from containers to the internet is translated by the host kernel using **Network Address Translation (NAT) and IP Masquerading**. Inbound traffic from external clients is directed to container ports via Linux `iptables` port forwarding rules.

### 2.2 The `host` Driver (Zero-Abstraction Raw Performance)
- **What it is:** A driver that completely disables container network isolation.
- **Working Mechanism:** The container does not receive its own private IP address or isolated network namespace. Instead, it binds directly to the host machine's physical network stack, sharing the host's IP address and port space.
- **Performance:** Eliminates all packet translation, veth switching overhead, and iptables NAT inspection, delivering bare-metal network throughput and ultra-low latency (identical to a native host process).
- **Caveat:** Port conflicts can occur. If the container binds to port `8080`, no other process on the host machine can use port `8080`.

### 2.3 The `none` Driver (Air-Gapped Absolute Isolation)
- **What it is:** Completely isolates the container from all network interfaces.
- **Working Mechanism:** The container receives an isolated network namespace containing **only the loopback interface (`127.0.0.1`)**. It possesses no external Ethernet adapters, no IP address, and no route to the host or internet.
- **Use Cases:** Ideal for running untrusted third-party code, cryptographic key generation, air-gapped batch mathematical simulations, or strict compliance tasks where network exfiltration must be physically impossible.

### 2.4 The `overlay` Driver (Multi-Host Distributed Mesh)
- **What it is:** Enables multi-host inter-container networking across multiple physical or cloud virtual machines without requiring host-level port mapping.
- **Working Mechanism:** Uses **Virtual Extensible LAN (VXLAN)** tunneling over UDP port 4789. It encapsulates layer-2 Ethernet frames from container traffic inside layer-4 UDP packets, routing them transparently across the underlying host network.
- **Primary Role:** Serves as the native networking engine for Docker Swarm clusters and multi-node container architectures.

### 2.5 The `macvlan` and `ipvlan` Drivers (Direct Physical LAN Integration)
- **What it is:** Connects container virtual interfaces directly to the physical host Ethernet adapter.
- **Working Mechanism:** Assigns each container its own unique physical MAC address and an individual, routable IP address directly from the physical network's local subnet (e.g., `192.168.1.50/24`).
- **Use Cases:** Ideal for legacy enterprise systems that monitor network traffic by physical IP/MAC addresses, network intrusion detection appliances, or VoIP telecommunications software that cannot tolerate NAT hops.

---

## 📊 3. Network Driver Comparison Matrix

The table below contrasts the operational characteristics of each native network driver:

| Driver Name | Network Isolation Level | Multi-Host Routing? | Requires Port Publishing (`-p`)? | Relative Network Latency | Primary Enterprise Scenario |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **`bridge`** | High (Virtual Switch) | No (Single Host) | Yes | Low (Minor NAT overhead) | Standard standalone microservices, web apps, and databases. |
| **`host`** | None (Uses Host Stack) | Yes (Via Host IP) | No (Binds directly) | Ultra-Low (Raw bare-metal line rate) | High-throughput streaming, gaming servers, network diagnostic tools. |
| **`none`** | Complete (Air-Gapped) | No | No (No network) | N/A | Secure offline computations, key generation, batch processing. |
| **`overlay`** | High (Encapsulated) | Yes (Multi-Host Mesh) | Optional (Overlay mesh routing) | Medium (VXLAN encapsulation hop) | Multi-node Docker Swarm clusters and cloud VM meshes. |
| **`macvlan`** | Medium (Physical LAN) | Yes (Via physical router) | No (Has own LAN IP) | Low (Direct hardware switching) | Legacy enterprise migrations, hardware device emulation. |

---

## 🛠️ 4. Docker Network Administrative CLI

The `docker network` CLI subsystem governs the creation, inspection, and lifecycle of virtual networks:

```bash
# 1. Create a custom user-defined bridge network with explicit subnet
docker network create --driver bridge --subnet 10.200.1.0/24 production_net

# 2. List all active networks on the host
docker network ls

# 3. Inspect subnet, gateway, and attached container IPs
docker network inspect production_net

# 4. Connect a running container to an existing network dynamically
docker network connect production_net api_container

# 5. Disconnect a container from a network without stopping it
docker network disconnect production_net api_container

# 6. Delete an unused network
docker network rm production_net

# 7. Prune all unused networks to reclaim resources
docker network prune -f
```

---

## 📋 5. Summary and Navigation

The Container Network Model and its pluggable driver architecture provide the operational flexibility required to run modern distributed applications. By selecting the `bridge` driver for isolated local services, `host` for line-rate throughput, `none` for air-gapped security, and `overlay` for distributed Swarm nodes, DevOps engineers construct resilient, secure network fabrics tailored to their workloads.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Docker-Storage-and-Volumes)](../04-Docker-Storage-and-Volumes/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Bridge Networking and Veth Pairs →](./02-Bridge-Networking-and-Veth-Pairs.md) |
