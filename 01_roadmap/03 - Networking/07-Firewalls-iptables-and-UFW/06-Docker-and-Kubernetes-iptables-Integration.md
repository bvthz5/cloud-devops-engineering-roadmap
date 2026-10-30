# 06 - Docker and Kubernetes iptables Integration

## 1. The Notorious Docker UFW Bypass Bug

When you start Docker with `-p 8080:80`, Docker injects rules directly into the Netfilter `nat` and `filter` tables under custom chains: `DOCKER` and `DOCKER-INGRESS`.

### The Security Vulnerability
If an engineer has UFW enabled with `ufw default deny incoming`, they assume port 8080 is blocked from the world.
**It is NOT!** Docker installs rules into the `PREROUTING` and `FORWARD` chains **before** UFW's rules are evaluated. The container port is exposed directly to the public internet!

### The Proper Fix: `DOCKER-USER` Chain
Docker reserves a special chain called `DOCKER-USER` that is evaluated **before** Docker's own forward rules. Add your firewall filtering here:

```bash
# Block all external traffic to published container ports except from trusted IP
iptables -I DOCKER-USER -i eth0 -s 203.0.113.50 -j ACCEPT
iptables -A DOCKER-USER -i eth0 -j DROP
```

---

## 2. Kubernetes kube-proxy iptables Mode

In standard Kubernetes clusters, `kube-proxy` configures thousands of iptables rules on every node to implement ClusterIP, NodePort, and LoadBalancer routing.

```
Incoming Packet (destined for ClusterIP: 10.96.0.10:80)
   │
   ▼
[PREROUTING] / [OUTPUT]
   │
   ▼
[KUBE-SERVICES] ────► Matches ClusterIP:Port
   │
   ▼
[KUBE-SVC-XXXXX] ────► Random probability distribution (Round-Robin load balancing)
   │
   ▼
[KUBE-SEP-YYYYY] ────► DNAT rewritten to Pod IP (e.g. 10.244.1.45:8080)
```

### Performance Scalability Limit: Why Cilium/eBPF Replaces iptables
At 5,000+ Services and 100,000+ Pods, iptables degrades severely because rule traversal is sequential `O(n)`. Every packet must scan thousands of lines. Modern clouds migrate to **Cilium / eBPF** or **IPVS** mode which offers `O(1)` hash-table lookup speed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Host Firewalls UFW and firewalld Management](./05-Host-Firewalls-UFW-and-firewalld-Management.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
