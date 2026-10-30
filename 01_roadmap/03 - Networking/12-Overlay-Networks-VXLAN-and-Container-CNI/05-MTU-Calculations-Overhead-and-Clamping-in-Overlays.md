# 05 - MTU Calculations, Overhead, and Clamping in Overlays

## 1. The MTU Encapsulation Tax

Standard Ethernet underlay networks use an MTU of **1500 bytes**.
When an overlay protocol wraps an inner packet, it adds outer headers:

| Protocol | Encapsulation Overhead | Recommended Pod MTU (on 1500 Underlay) |
|---|---|---|
| **VXLAN** | 50 Bytes (14B Eth + 20B IP + 8B UDP + 8B VXLAN) | **1450 Bytes** |
| **Geneve** | 50 to 72 Bytes | **1430 to 1450 Bytes** |
| **IPsec (in Calico/WireGuard)** | 60 to 80 Bytes | **1420 to 1440 Bytes** |
| **AWS VPC CNI (Native)** | 0 Bytes (No encapsulation!) | **9001 Bytes (Jumbo Frames supported)** |

---

## 2. The Silent Hang Failure Mode

If a Kubernetes Pod is configured with `MTU = 1500` on a VXLAN network:
1. TCP 3-way handshake succeeds (SYN packets are only ~60 bytes).
2. Small API requests (`GET /`) succeed.
3. Client attempts to upload an image or database query returning a 4 KB payload.
4. Pod transmits a 1500-byte packet.
5. Node attempts to add 50 bytes of VXLAN header $ightarrow$ Total packet becomes **1550 bytes**!
6. Underlying cloud network drops the packet because it exceeds physical MTU 1500.
7. Connection **freezes indefinitely** with zero error message!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Comparing Major CNIs Flannel Calico Cilium AWS VPC CNI](./04-Comparing-Major-CNIs-Flannel-Calico-Cilium-AWS-VPC-CNI.md) | [Index](../../../README.md) | [06 - Network Policies and Micro Segmentation →](./06-Network-Policies-and-Micro-Segmentation.md) |
