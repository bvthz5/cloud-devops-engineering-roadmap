# 08 - Firewalls & iptables: Troubleshooting Guide

## 1. Quick Diagnostic Tree

```
Packet dropped or connection timing out?
  │
  ├── 1. Check Cloud Hypervisor Firewalls (AWS Security Groups, GCP Firewall Rules)
  │      └── Are packets reaching the host at all? (tcpdump -i eth0 port 443)
  │
  ├── 2. Inspect Conntrack Table Exhaustion
  │      ├── cat /proc/sys/net/netfilter/nf_conntrack_count
  │      └── cat /proc/sys/net/netfilter/nf_conntrack_max
  │
  ├── 3. Trace iptables Rule Counters
  │      └── sudo iptables -vnL | grep -E "DROP|REJECT"
  │
  └── 4. Check Logging Rules
         └── dmesg -T | grep -i "IPTABLES-DROP"
```

---

## 2. Essential Diagnostic Commands

### Trace Dropped Packets by Logging
Insert a temporary logging rule before the DROP target:
```bash
sudo iptables -I INPUT 1 -p tcp --dport 80 -j LOG --log-prefix "IPTABLES-IN-80: " --log-level 4
tail -f /var/log/kern.log | grep "IPTABLES-IN-80"
```

### Inspect Conntrack Sessions
```bash
# Install conntrack CLI tool
sudo apt-get install -y conntrack

# Display top 10 source IPs consuming state entries
conntrack -L | awk '{print $4}' | cut -d= -f2 | sort | uniq -c | sort -nr | head -n 10
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
