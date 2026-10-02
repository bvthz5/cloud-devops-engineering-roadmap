# 07 — Real-World Firewall Production Scenarios

Firewall configurations in modern cloud infrastructure interact directly with container runtimes, Kubernetes, and DDoS attacks. Here are three critical real-world production challenges.

---

## Scenario 1: The Docker UFW Bypass Vulnerability

### Incident Summary
A DevOps engineer configures UFW on an Ubuntu host with `ufw default deny incoming` and does NOT open port `8080`.
They launch a container: `docker run -d -p 8080:80 nginx`.
To their horror, port `8080` is **publicly accessible from the entire internet**, completely ignoring UFW rules!

### Root Cause Analysis
Docker manipulates `iptables` rules directly. When Docker publishes a port with `-p 8080:80`, it inserts `DNAT` rules into the `PREROUTING` chain and forwarding rules into the custom `DOCKER` chain in the `FORWARD` filter table.
Because packets destined for containers are routed to `FORWARD`, and Docker's rules are evaluated **before** UFW's rules in `FORWARD`, UFW's `INPUT` table policies are completely bypassed!

### Production Solution
Use the `DOCKER-USER` chain. Docker officially reserves this chain for administrator rules, and evaluates it before any Docker container rules:

```bash
# Block all external access to Docker containers except from trusted IP (198.51.100.50)
sudo iptables -I DOCKER-USER -i eth0 -j DROP
sudo iptables -I DOCKER-USER -i eth0 -s 198.51.100.50 -j ACCEPT
sudo iptables -I DOCKER-USER -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

---

## Scenario 2: Connection Tracking Table Full (`nf_conntrack: table full, dropping packet`)

### Incident Summary
During a high-concurrency marketing event or DDoS attack, an API server stops accepting new connections. System logs (`dmesg` or `journalctl -k`) show:
`kernel: nf_conntrack: table full, dropping packet`.

### Root Cause Analysis
Netfilter creates an entry in `/proc/net/nf_conntrack` for every tracked socket. When concurrent connections exceed `net.netfilter.nf_conntrack_max`, the kernel silently drops all new SYN packets.

### Production Solution
1. Check current usage vs maximum:
   ```bash
   sysctl net.netfilter.nf_conntrack_count
   sysctl net.netfilter.nf_conntrack_max
   ```
2. Double the connection tracking table size and hash bucket size:
   ```ini
   # /etc/sysctl.d/99-conntrack.conf
   net.netfilter.nf_conntrack_max = 524288
   net.netfilter.nf_conntrack_tcp_timeout_established = 600
   ```
3. For ultra-high-throughput load balancers (e.g. HAProxy/NGINX edge proxies), bypass connection tracking completely using the `raw` table:
   ```bash
   sudo iptables -t raw -A PREROUTING -p tcp --dport 80 -j NOTRACK
   sudo iptables -t raw -A PREROUTING -p tcp --dport 443 -j NOTRACK
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - NAT, Port Forwarding & Masquerading](./06-NAT-Port-Forwarding-and-Masquerading.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
