# 09 — Linux Firewalls Interview Q&A

10 technical interview questions for DevOps, SRE, and Cloud Security roles.

---

### Q1: What is the technical difference between `DROP` and `REJECT` targets?
**Answer:**
- `DROP`: Silently discards the packet. No notification is returned to the client. The sender will wait until their client-side timeout expires (typically 30–60 seconds). Ideal for public edge firewalls against port scanners and DDoS attacks because it leaks no information.
- `REJECT`: Discards the packet but actively sends an error packet back to the sender (a `TCP RST` for TCP, or an `ICMP Destination Unreachable / Port Unreachable` for UDP). Ideal for internal trusted networks because it allows legitimate client applications to fail immediately and gracefully without hanging.

---

### Q2: Why does Docker bypass standard host UFW rules on Ubuntu?
**Answer:**
Docker injects its own NAT and forwarding rules into the `FORWARD` chain of `iptables` to route packets from the physical interface into the `docker0` bridge. Because UFW's primary user rules are defined for the `INPUT` chain, packets routed directly through `FORWARD` bypass UFW's `INPUT` policies entirely. To restrict access to published container ports, administrators must insert rules into the `DOCKER-USER` chain.

---

### Q3: What is the function of the `raw` table in `iptables`?
**Answer:**
The `raw` table has the highest priority and is evaluated before connection tracking takes place. Its primary use case is to mark packets with the `NOTRACK` target (`-j NOTRACK`). This exempts high-volume traffic (such as DNS servers or CDN reverse proxies handling millions of requests) from `conntrack` state tracking, saving significant CPU memory and preventing conntrack table exhaustion.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
