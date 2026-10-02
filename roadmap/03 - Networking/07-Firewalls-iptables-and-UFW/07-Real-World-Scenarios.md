# 07 - Firewalls & iptables: Real-World Production Scenarios

## Scenario 1: Black Friday Checkout Meltdown (Conntrack Table Full)

### Incident Summary
During an e-commerce flash sale, customer transactions spiked by 12x. Within 4 minutes, all API calls to the Kubernetes ingress gateway failed with `Connection timed out`. Synthetic health checks to the nodes failed, although CPU and memory usage were under 35%.

### Root Cause
The Kubernetes worker nodes were acting as NAT gateways for container egress. The kernel connection tracking parameter `net.netfilter.nf_conntrack_max` was set to the default value of `262,144`. The massive volume of short-lived database queries and API calls saturated the conntrack table, triggering the kernel to execute:
```
nf_conntrack: table full, dropping packet
```

### Resolution & Mitigation
1. Immediately doubled conntrack capacity dynamically:
   ```bash
   sudo sysctl -w net.netfilter.nf_conntrack_max=1048576
   ```
2. Shortened the default TCP TIME_WAIT connection retention from 120s to 30s.
3. Implemented connection pooling across microservices to reuse existing TCP connections.

---

## Scenario 2: Data Breach via Docker Default Port Publishing

### Incident Summary
A staging Redis container containing production-sanitized client tokens was launched on an AWS EC2 instance:
```bash
docker run -d -p 6379:6379 redis:latest
```
Although UFW was configured to deny all incoming traffic except SSH, an automated Shodan scanner detected and dumped the unauthenticated Redis database within 2 hours.

### Root Cause
Docker binds to `0.0.0.0:6379` by default and manipulates Netfilter's `PREROUTING` chain, completely bypassing UFW's `INPUT` chain filters.

### Resolution & Mitigation
1. Updated Docker daemon configuration (`/etc/docker/daemon.json`) to bind only to localhost by default:
   ```json
   { "ip": "127.0.0.1" }
   ```
2. Hardened AWS EC2 Security Groups at the hypervisor level (Security Groups intercept packets before they reach the Linux host OS Netfilter stack).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Docker and Kubernetes iptables Integration](./06-Docker-and-Kubernetes-iptables-Integration.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
