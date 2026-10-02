# 10 — Hands-On Practice Labs: Linux Firewalls

Practical labs to master Linux firewall configuration.

---

## Lab 1: Bulletproof Server Firewall with UFW

### Objective
Secure an Ubuntu server with a zero-trust incoming policy, open standard web ports, and rate-limit SSH to prevent brute-force attacks.

```bash
# 1. Reset and set zero-trust baseline
sudo ufw reset
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 2. Allow SSH with rate limiting
sudo ufw limit 22/tcp comment 'Rate limited SSH'

# 3. Allow Web traffic
sudo ufw allow 80/tcp comment 'HTTP'
sudo ufw allow 443/tcp comment 'HTTPS'

# 4. Enable and verify
sudo ufw --force enable
sudo ufw status verbose
```

---

## Lab 2: Modern nftables Stateful Firewall with Sets

### Objective
Configure an atomic stateful firewall ruleset using `nftables` sets.

Create `/tmp/lab-firewall.nft`:
```nft
table inet filter {
    set web_ports {
        type inet_service
        elements = { 80, 443 }
    }

    chain input {
        type filter hook input priority 0; policy drop;

        iif "lo" accept
        ct state established,related accept
        ct state invalid drop

        tcp dport 22 accept
        tcp dport @web_ports accept
    }
}
```
Apply ruleset:
```bash
sudo nft -f /tmp/lab-firewall.nft
sudo nft list ruleset
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
