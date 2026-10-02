# 01 - Linux Netfilter Architecture and Kernel Hooks

## 1. What is Netfilter?

**Netfilter** is a framework inside the Linux kernel that provides hooks for packet filtering, network address translation (NAT), packet mangling, and connection tracking. 

Software like `iptables`, `nftables`, `ufw`, `firewalld`, Docker, and `kube-proxy` do not filter packets directly in user space; they are user-space configuration frontends that program rules into the kernel's Netfilter subsystem.

```
+---------------------------------------------------------------------------------------+
|                                    LINUX KERNEL                                       |
|                                                                                       |
|   Network Interface                                               Network Interface   |
|         (eth0)                                                         (eth1)         |
|           |                                                               ▲           |
|           ▼                                                               |           |
|     [PREROUTING] ────► [Routing Decision] ────► [FORWARD] ────► [POSTROUTING]         |
|                             │                                         ▲               |
|                             ▼                                         │               |
|                          [INPUT]                                  [OUTPUT]            |
|                             │                                         ▲               |
|                             ▼                                         │               |
|                     Local Process / App                     Local Process / App       |
|                     (e.g., NGINX, sshd)                     (e.g., curl, python)      |
+---------------------------------------------------------------------------------------+
```

---

## 2. The 5 Netfilter Kernel Hooks

Every IP packet traversing a Linux machine hits specific kernel hooks depending on its source and destination:

| Hook | When it is Triggered | Typical Use Cases |
|---|---|---|
| `NF_INET_PRE_ROUTING` | Immediately after the NIC driver parses the packet, before any routing decision is made. | Destination NAT (DNAT), packet defragmentation, raw state bypassing. |
| `NF_INET_LOCAL_IN` | After routing decision determines the packet is destined for a local socket/process on this host. | Local firewall filtering (allow SSH, drop unauthorized inbound traffic). |
| `NF_INET_FORWARD` | After routing decision determines the packet is destined for another host (this host is acting as a router/gateway/bridge). | Routed container traffic (Docker bridge to outside, Kubernetes Pod-to-Pod). |
| `NF_INET_LOCAL_OUT` | Triggered by packets generated locally by a local process before routing. | Outbound firewall rules, mangling outbound traffic marks. |
| `NF_INET_POST_ROUTING` | Triggered by any outbound packet (locally generated or forwarded) just before it hits the NIC driver. | Source NAT (SNAT), IP Masquerade (`MASQUERADE`). |

---

## 3. Packet Traversal Life Cycle

### Scenario A: Incoming Packet Destined for Local Server (e.g. Inbound HTTPS to NGINX)
1. Packet arrives at NIC (`eth0`).
2. Netfilter hook: `PREROUTING` (Connection tracking assigned, DNAT applied if any).
3. Kernel makes **Routing Decision**: Packet destination IP matches local IP `192.168.1.50`.
4. Netfilter hook: `INPUT` (Firewall checks: Is port 443 allowed? Yes -> ACCEPT).
5. Packet delivered to local listening socket (NGINX daemon).

### Scenario B: Forwarded Packet (e.g. Docker Container to Internet via Host Gateway)
1. Packet arrives from container interface `veth123` into bridge `docker0`.
2. Netfilter hook: `PREROUTING`.
3. Kernel makes **Routing Decision**: Destination IP is `8.8.8.8` (Remote). Packet must be forwarded out `eth0`.
4. Netfilter hook: `FORWARD` (Firewall checks: Is container allowed to send traffic out? Yes -> ACCEPT).
5. Netfilter hook: `POSTROUTING` (SNAT / MASQUERADE replaces container private IP `172.17.0.2` with host public IP `54.210.10.5`).
6. Packet leaves host physical NIC `eth0`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-SSH-and-Secure-Remote-Access)](../06-SSH-and-Secure-Remote-Access/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - iptables Tables Chains and Rule Syntax →](./02-iptables-Tables-Chains-and-Rule-Syntax.md) |
