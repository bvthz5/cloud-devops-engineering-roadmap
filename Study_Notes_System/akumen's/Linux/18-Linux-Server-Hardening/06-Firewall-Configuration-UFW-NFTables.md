# 6. Firewall Configuration (UFW & nftables)

## Host-Based Firewall Strategy
Implement a **Default Deny** incoming policy: block all inbound connections by default, then explicitly allow required ports.

## Uncomplicated Firewall (UFW) - Debian / Ubuntu

### Setup & Basic Rules
```bash
# Reset UFW to default state
sudo ufw reset

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow custom SSH port (e.g., 2222)
sudo ufw allow 2222/tcp comment 'SSH Port'

# Allow HTTP and HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow specific IP address to access database port 5432
sudo ufw allow from 192.168.1.50 to any port 5432 proto tcp

# Enable firewall
sudo ufw enable

# Check status with numbered rules
sudo ufw status verbose
```

## nftables / iptables - RHEL / Enterprise Linux

### Basic `nftables` commands:
```bash
# Enable nftables service
sudo systemctl enable --now nftables

# List current ruleset
sudo nft list ruleset
```
