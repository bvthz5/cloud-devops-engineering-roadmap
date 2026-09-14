# 11. Kernel Hardening with Sysctl

`sysctl` modifies Linux kernel parameters at runtime.

## Hardening File: `/etc/sysctl.d/99-security-hardening.conf`

```ini
# Disable IP packet forwarding (unless server is a router/NAT)
net.ipv4.ip_forward = 0

# Disable ICMP redirect acceptance (prevents MITM route hijacking)
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

# Enable SYN Cookies (mitigates SYN Flood Denial-of-Service attacks)
net.ipv4.tcp_syncookies = 1

# Ignore ICMP echo requests (ping requests - optional)
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Enable Reverse Path Filtering (prevents IP spoofing)
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1

# Log impossible or spoofed packets
net.ipv4.conf.all.log_martians = 1

# Restrict access to kernel pointer addresses (kptr_restrict)
kernel.kptr_restrict = 2

# Restrict dmesg kernel log access to root
kernel.dmesg_restrict = 1
```

Apply kernel settings:
```bash
sudo sysctl -p /etc/sysctl.d/99-security-hardening.conf
```
