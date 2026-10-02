# 05 - Host Firewalls: UFW and firewalld Management

## 1. UFW (Uncomplicated Firewall) on Ubuntu/Debian

UFW is an intuitive management frontend for `iptables`/`nftables` designed to eliminate configuration errors.

```bash
# Check UFW status and active rules
sudo ufw status verbose

# Set secure baseline defaults (Deny incoming, Allow outgoing)
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH before enabling!
sudo ufw allow 22/tcp comment "SSH Management"

# Allow specific IP or subnet to access SSH
sudo ufw allow from 192.168.1.100 to any port 22 proto tcp

# Allow HTTP and HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Delete rule by number
sudo ufw status numbered
sudo ufw delete 3

# Enable or reload UFW
sudo ufw enable
sudo ufw reload
```

---

## 2. firewalld on RHEL / CentOS / Rocky Linux

`firewalld` uses dynamically managed **Zones** to assign trust levels to network connections or interfaces.

| Zone | Default Behavior |
|---|---|
| `drop` | Drops all incoming packets with zero reply. |
| `block` | Rejects all incoming packets with ICMP destination-unreachable. |
| `public` | Default zone. Accept selected incoming services (SSH). |
| `internal` | High trust. For private VPC communications. |
| `trusted` | All network connections accepted. |

```bash
# Check active zones and rules
firewall-cmd --get-active-zones
firewall-cmd --list-all

# Allow HTTPS permanently in public zone
firewall-cmd --zone=public --add-service=https --permanent

# Allow custom port range
firewall-cmd --zone=public --add-port=8080-8085/tcp --permanent

# Reload configuration without dropping existing connections
firewall-cmd --reload
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - nftables](./04-nftables-Modern-Linux-Packet-Classification.md) | [README](./README.md) | [06 - Docker & Kubernetes iptables](./06-Docker-and-Kubernetes-iptables-Integration.md) |
