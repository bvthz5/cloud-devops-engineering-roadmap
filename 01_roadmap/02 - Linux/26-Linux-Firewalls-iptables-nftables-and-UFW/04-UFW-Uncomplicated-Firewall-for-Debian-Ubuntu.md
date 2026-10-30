# 04 — UFW (Uncomplicated Firewall) for Debian and Ubuntu

UFW is the standard user-friendly firewall interface on Ubuntu and Debian systems. It simplifies complex `iptables` and `nftables` rules into clean, human-readable commands while remaining powerful enough for production servers.

---

## 1. Quick-Start Production Hardening

```bash
# 1. Check current status
sudo ufw status verbose

# 2. Set default policies (DROP incoming, ALLOW outgoing)
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 3. CRITICAL: Allow SSH BEFORE enabling, otherwise you will lock yourself out!
sudo ufw allow 22/tcp comment 'OpenSSH remote admin'

# 4. Enable the firewall
sudo ufw enable

# 5. Verify active rules with numbered index
sudo ufw status numbered
```

---

## 2. Common UFW Rule Patterns

```bash
# Allow specific ports
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow port ranges
sudo ufw allow 30000:32767/tcp comment 'Kubernetes NodePort range'

# Allow traffic only from a specific IP address
sudo ufw allow from 203.0.113.50 to any port 22 proto tcp

# Allow traffic from an entire CIDR subnet to PostgreSQL
sudo ufw allow from 10.0.1.0/24 to any port 5432 proto tcp

# Deny a malicious IP address completely
sudo ufw deny from 198.51.100.25

# Rate-limiting SSH (protects against brute-force attacks)
# Automatically blocks IPs that attempt 6 or more connections within 30 seconds
sudo ufw limit 22/tcp

# Delete a rule by its number
sudo ufw status numbered
sudo ufw delete 2

# Reset UFW to factory defaults (disables and removes all rules)
sudo ufw reset
```

---

## 3. UFW Application Profiles

UFW can read pre-configured application profiles located in `/etc/ufw/applications.d/`:

```bash
# List available application profiles
sudo ufw app list

# Inspect what ports a profile opens
sudo ufw app info 'Nginx Full'

# Allow using profile name
sudo ufw allow 'Nginx Full'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - nftables The Modern Linux Firewall](./03-nftables-The-Modern-Linux-Firewall.md) | [Index](../../../README.md) | [05 - firewalld Dynamic Firewall for RHEL Rocky →](./05-firewalld-Dynamic-Firewall-for-RHEL-Rocky.md) |
