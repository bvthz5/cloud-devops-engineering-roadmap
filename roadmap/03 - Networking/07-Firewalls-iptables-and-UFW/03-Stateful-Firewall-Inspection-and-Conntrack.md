# 03 - Stateful Firewall Inspection and Conntrack

## 1. Stateless vs. Stateful Firewalls

- **Stateless Firewall (e.g. AWS Network ACLs):** Inspects each packet in complete isolation. If you allow inbound port 80, you MUST also explicitly create an outbound rule allowing ephemeral ports (1024-65535) for response traffic.
- **Stateful Firewall (e.g. Linux iptables conntrack, AWS Security Groups):** Tracks bidirectional conversation flows. Once an inbound request is permitted, response packets are automatically recognized and permitted as part of the established connection.

---

## 2. Connection Tracking States (`conntrack`)

Netfilter tracks connections through four primary states:

| State | Definition |
|---|---|
| `NEW` | The packet is initiating a new connection (e.g., initial TCP SYN packet). |
| `ESTABLISHED` | The packet belongs to a connection that has seen bidirectional traffic (TCP handshake complete). |
| `RELATED` | The packet is initiating a new connection associated with an existing connection (e.g., FTP data channels, ICMP error messages caused by an active TCP session). |
| `INVALID` | The packet cannot be associated with any known connection and does not identify a valid new state (often dropped immediately). |

```bash
# Gold standard base rule for every production Linux server
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```

---

## 3. The Conntrack Table and Sizing Disasters

The Linux kernel maintains active connection state records in memory:
- View current connections: `conntrack -L` or `cat /proc/net/nf_conntrack`
- View current connection count: `cat /proc/sys/net/netfilter/nf_conntrack_count`
- View maximum allowable connections: `cat /proc/sys/net/netfilter/nf_conntrack_max`

### The Outage: `nf_conntrack: table full, dropping packet`
When high-traffic microservices, Kubernetes nodes, or NAT gateways hit `nf_conntrack_max`, the Linux kernel **drops all new incoming packets immediately** without notice!

```
# Check kernel logs for conntrack exhaustion:
dmesg -T | grep -i conntrack
[Thu Oct 01 14:02:11 2026] nf_conntrack: table full, dropping packet
```

### Production Sizing Formula
For high-throughput servers (64GB RAM+):
```bash
# Set max connections to 2 million
sysctl -w net.netfilter.nf_conntrack_max=2097152

# Sizing hash buckets: hashsize = nf_conntrack_max / 4
echo 524288 > /sys/module/nf_conntrack/parameters/hashsize

# Persist in /etc/sysctl.d/99-conntrack.conf
net.netfilter.nf_conntrack_max = 2097152
net.netfilter.nf_conntrack_tcp_timeout_established = 86400
net.netfilter.nf_conntrack_tcp_timeout_close_wait = 60
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - iptables Architecture](./02-iptables-Tables-Chains-and-Rule-Syntax.md) | [README](./README.md) | [04 - nftables](./04-nftables-Modern-Linux-Packet-Classification.md) |
