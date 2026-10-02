# 09 - Firewalls & iptables: Interview Questions & Answers

### Q1: What is the architectural difference between Netfilter, iptables, and UFW?
**Answer:** **Netfilter** is the kernel-level packet filtering and NAT engine. **iptables** is the user-space command-line tool used to configure Netfilter's tables and chains. **UFW** is a high-level wrapper program designed to manage iptables/nftables rules with simplified command syntax.

### Q2: What happens when `nf_conntrack_max` is reached?
**Answer:** The Linux kernel refuses to track any new connections. All incoming packets with state `NEW` are immediately and silently dropped. Existing connections marked `ESTABLISHED` will typically survive until they time out or send new out-of-order packets.

### Q3: Why does Docker bypass standard UFW firewall rules on an Ubuntu host?
**Answer:** UFW configures the `INPUT` chain in the `filter` table. Docker uses NAT and forwards traffic into the `FORWARD` chain, attaching its custom `DOCKER` chains directly into the Netfilter engine before UFW's rules are reached.

### Q4: Explain the difference between `DROP` and `REJECT` targets.
**Answer:** `DROP` discards the packet silently. The sender receives no feedback and must wait for client connection timeouts (useful against scanning/DDoS). `REJECT` discards the packet but sends back an explicit error code (e.g. TCP RST or ICMP Port Unreachable), allowing clients to fail fast.

### Q5: What is the purpose of the `raw` table in iptables?
**Answer:** The `raw` table is evaluated before any connection tracking occurs. It is primarily used to mark high-volume packets with the `NOTRACK` target (e.g., DNS root servers or 100k req/sec load balancers) to avoid exhausting the conntrack state table.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
