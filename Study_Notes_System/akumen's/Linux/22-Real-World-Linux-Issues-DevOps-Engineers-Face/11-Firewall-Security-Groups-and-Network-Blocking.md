# 11 — Firewall, Security Groups, and Network Blocking

## 1. Scenario
Clients cannot reach port 443 on web server. Server process is active (`ss -tulpn` shows `nginx` listening on `0.0.0.0:443`), but remote requests time out.

```text
Scenario
   ↓
Symptoms: Connection timed out when accessing remote port
   ↓
What could cause it? Local UFW / iptables firewall rule blocking port, AWS Security Group missing ingress rule
   ↓
Diagnostic commands: nc -zv host 443, ufw status, iptables -L -n -v, traceroute -T -p 443 host
```

## 2. Testing Network Connectivity (`netcat` / `nc`)

```bash
# Test TCP connection timeout (fast zero-I/O check)
nc -zv -w 3 192.168.1.50 443
```
- If output is `Connection refused`: Service process is down / not listening on port.
- If output is `Connection timed out`: Packet is being dropped silently by firewall or security group!

## 3. Local Linux Firewall Inspection

### UFW (Ubuntu)
```bash
sudo ufw status verbose
# Fix: Allow port 443
sudo ufw allow 443/tcp
```

### IPTables
```bash
sudo iptables -L -n -v --line-numbers
# Fix: Insert ACCEPT rule at line 1
sudo iptables -I INPUT 1 -p tcp --dport 443 -j ACCEPT
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - SSH Debugging](./10-SSH-Connectivity-Auth-and-Config-Troubleshooting.md) | [README](./README.md) | [12 - DNS Resolution Failures](./12-DNS-Resolution-Failures-and-Diagnostic-Tools.md) |
