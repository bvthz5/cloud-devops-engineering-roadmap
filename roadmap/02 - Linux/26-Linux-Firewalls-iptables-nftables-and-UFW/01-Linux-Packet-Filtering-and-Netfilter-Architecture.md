# 01 — Linux Packet Filtering and Netfilter Architecture

At the heart of all Linux firewall technologies (`iptables`, `nftables`, `ufw`, `firewalld`) lies **Netfilter**, a framework within the Linux kernel that provides hooks for packet inspection, modification, and dropping.

---

## 1. The 5 Kernel Netfilter Hooks

As a packet moves through the Linux network stack, it encounters five predefined hook points:

```text
Incoming Packet
       ↓
[ 1. PREROUTING ] ------> (Routing Decision: Destined for local host or forward?)
       |                                      |
       | (Local Host)                         | (Forward to another host)
       v                                      v
[ 2. INPUT ]                           [ 3. FORWARD ]
       |                                      |
       v                                      |
Local Process (e.g. NGINX)                    |
       |                                      |
       v                                      |
[ 4. OUTPUT ]                                 |
       |                                      |
       +-----------------> [ 5. POSTROUTING ] <+
                                  |
                                  v
                            Outgoing Packet
```

1. **`NF_INET_PRE_ROUTING` (PREROUTING):** Triggered immediately after the packet enters the network interface and before any routing decision is made. Used primarily for Destination NAT (DNAT).
2. **`NF_INET_LOCAL_IN` (INPUT):** Triggered if the packet is addressed to a local IP address on this host. Used for host firewall protection.
3. **`NF_INET_FORWARD` (FORWARD):** Triggered if the packet is destined for another host (e.g., when the host acts as a router, bridge, or Docker container host).
4. **`NF_INET_LOCAL_OUT` (OUTPUT):** Triggered when a packet is generated locally by a process on this host (e.g., `curl` making an outbound request).
5. **`NF_INET_POST_ROUTING` (POSTROUTING):** Triggered after the routing decision, right before the packet is placed on the wire. Used for Source NAT (SNAT) and Masquerading.

---

## 2. Netfilter Connection Tracking (`conntrack`)

Stateful firewalls remember active network conversations. The Netfilter `conntrack` module tracks every packet and assigns it one of four states:

- **`NEW`:** The packet is requesting a new connection (e.g., TCP SYN packet).
- **`ESTABLISHED`:** The packet belongs to an existing, already approved bidirectional connection (e.g., after the 3-way handshake completes).
- **`RELATED`:** The packet is starting a new connection that is auxiliary to an existing connection (e.g., FTP data channels, ICMP error messages).
- **`INVALID`:** The packet cannot be identified or does not follow valid protocol state sequences (e.g., out-of-order flags, corrupted checksums). Usually dropped immediately.

The table of active tracked connections lives in memory and can be inspected via `/proc/net/nf_conntrack` or the `conntrack` CLI:
```bash
sudo conntrack -L
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (25-Linux-Networking-and-DNS-Troubleshooting)](../25-Linux-Networking-and-DNS-Troubleshooting/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - iptables Deep Dive and Rule Management →](./02-iptables-Deep-Dive-and-Rule-Management.md) |
