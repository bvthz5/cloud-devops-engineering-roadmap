# 01 — Linux Network Stack and Device Model

In Linux, networking is deeply embedded in the kernel. The kernel provides device driver abstractions, network protocol implementations (TCP/IPv4, IPv6, UDP, ICMP), queuing disciplines (qdisc), and packet filtering frameworks (Netfilter).

---

## 1. The Kernel Network Architecture

When a physical network packet arrives at a Network Interface Card (NIC):
```text
+-----------------------------------------------------------------+
| Physical NIC (Hardware)                                         |
|  - Receives electrical/optical signal or radio wave             |
|  - Validates Ethernet CRC checksum                              |
|  - DMA (Direct Memory Access) transfers frame into Ring Buffer  |
|  - Raises Hardware Interrupt (IRQ)                              |
+-----------------------------------------------------------------+
                               ↓
+-----------------------------------------------------------------+
| Kernel Driver & NAPI (New API) Subsystem                        |
|  - CPU schedules SoftIRQ (NET_RX_SOFTIRQ)                       |
|  - Polls ring buffer without per-packet hardware interrupts     |
|  - Allocates `sk_buff` (Socket Buffer) kernel memory structure  |
+-----------------------------------------------------------------+
                               ↓
+-----------------------------------------------------------------+
| Network Layer (Layer 3 - IPv4 / IPv6)                           |
|  - Netfilter PREROUTING hooks (iptables / nftables)             |
|  - Route Lookup: Local destination or forward to another host?  |
|  - Netfilter INPUT hooks                                        |
+-----------------------------------------------------------------+
                               ↓
+-----------------------------------------------------------------+
| Transport Layer (Layer 4 - TCP / UDP / ICMP)                    |
|  - Port demultiplexing, sequence validation, TCP state machine  |
|  - Packet placed into Socket Receive Buffer (`sk_rcvbuf`)       |
+-----------------------------------------------------------------+
                               ↓
+-----------------------------------------------------------------+
| User Space Application (NGINX, Python, Go, Node.js)             |
|  - Process awakened via epoll / select / poll / io_uring        |
|  - Executes `read()` or `recv()` system call                    |
+-----------------------------------------------------------------+
```

---

## 2. Linux Network Interface Types

Linux treats network interfaces as software devices represented in `/sys/class/net/`:

| Device Type | Prefix Example | Purpose / Use Case |
| :--- | :--- | :--- |
| **Physical Ethernet** | `eth0`, `ens3`, `enp0s3` | Physical network cables or cloud VM virtual NICs (SR-IOV, VirtIO, ENA). |
| **Loopback** | `lo` | `127.0.0.1` / `::1`. Inter-process communication on the local machine without hardware. |
| **Virtual Ethernet** | `veth` (`veth0`, `veth1`)| Always created in pairs. Acts as a virtual patch cord connecting two network namespaces or a container to a host bridge. |
| **Linux Bridge** | `docker0`, `br0`, `cbr0` | Software Layer 2 switch. Connects multiple virtual or physical interfaces together. |
| **TUN / TAP** | `tun0`, `tap0` | User-space network tunnels (VPNs like OpenVPN, WireGuard, and hypervisors like QEMU/KVM). TUN operates at L3 (IP), TAP at L2 (Ethernet). |
| **VLAN Interface** | `eth0.100` | 802.1Q tagged sub-interface for network segmentation. |
| **Bonding / Team** | `bond0` | Aggregation of multiple physical NICs for high-availability failover or link aggregation (LACP). |

---

## 3. MTU (Maximum Transmission Unit) & Jumbo Frames

- **Standard Ethernet MTU:** `1500 bytes`. This is the maximum Layer 3 payload that can be transmitted in a single Ethernet frame without IP fragmentation.
- **Jumbo Frames:** Typically `9000 bytes`. Used in cloud private VPCs (e.g. AWS Nitro VPCs, SAN storage networks) to reduce CPU overhead by sending larger payloads per interrupt.
- **Geneve / VXLAN Overhead:** Container overlays (e.g., Flannel, Calico VXLAN) encapsulate packets, adding 50 bytes of header. If host MTU is 1500, pod MTU must be set to `1450` to prevent fragmentation.

---

## 4. Hardware Offloading (ethtool)

Modern NICs offload expensive CPU tasks to dedicated silicon:
- **TSO (TCP Segmentation Offload):** Kernel passes large packets (up to 64KB) to the NIC, and the NIC breaks them down into MTU-sized frames.
- **GRO (Generic Receive Offload):** NIC or driver merges sequential incoming TCP packets into a single large packet before delivering to the kernel.
- **Checksum Offload (`rx-checksumming`, `tx-checksumming`):** Hardware calculates and verifies TCP/UDP checksums.

```bash
# View offload settings
ethtool -k eth0

# Disable offloads (useful when debugging checksum corruption or packet captures)
sudo ethtool -K eth0 tso off gso off gro off
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Modern Network Configuration](./02-Modern-Network-Configuration-iproute2.md) |
