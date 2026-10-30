# 08 — Linux Firewall Troubleshooting Guide & Runbook

Diagnosing firewall issues requires tracking whether packets are dropped, rejected, or misrouted.

---

## 1. Step-by-Step Firewall Diagnostic Procedure

1. **Verify iptables Drop Counters:**
   ```bash
   sudo iptables -L -n -v | grep DROP
   ```
   If the packet counter increment is increasing when you attempt connection, your rule is dropping traffic.

2. **Add Temporary Logging Rules:**
   Insert a `LOG` rule immediately before your `DROP` policy:
   ```bash
   sudo iptables -I INPUT -p tcp --dport 80 -j LOG --log-prefix "FIREWALL-DROP: " --log-level 4
   ```
   Inspect dropped packets in real time:
   ```bash
   sudo journalctl -k -f | grep "FIREWALL-DROP"
   ```

3. **Check Connection Tracking Table (`conntrack`):**
   ```bash
   # Check if host is dropping packets due to table exhaustion
   sudo dmesg -T | grep -i conntrack
   ```

---

## 2. Emergency Firewall Recovery (Console Access)

If you lock yourself out of a remote server via SSH due to a faulty firewall rule, use your cloud provider's serial console or VNC:

```bash
# Emergency: Flush all rules and reset default policy to ACCEPT
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo ufw disable
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
