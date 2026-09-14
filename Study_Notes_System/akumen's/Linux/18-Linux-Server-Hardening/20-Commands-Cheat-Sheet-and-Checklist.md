# 20. Commands Cheat Sheet and Verification Checklist

## Hardening Commands Cheat Sheet

```bash
# Account Locking
sudo passwd -l username
sudo usermod -s /usr/sbin/nologin username

# SSH Configuration Validation
sudo sshd -t
sudo systemctl reload ssh

# UFW Firewall
sudo ufw default deny incoming
sudo ufw allow 22/tcp
sudo ufw enable

# Audit Listening Ports
sudo ss -tulpn

# SUID File Audit
sudo find / -xdev -type f -perm -4000

# Kernel Sysctl Reload
sudo sysctl --system
```

## Hardening Verification Checklist
- [ ] Direct `root` SSH login disabled (`PermitRootLogin no`)
- [ ] SSH Password Authentication disabled (`PasswordAuthentication no`)
- [ ] UFW/nftables default deny incoming policy enabled
- [ ] Unnecessary services stopped and disabled
- [ ] Critical permissions verified (`/etc/shadow` = 600)
- [ ] `sysctl` kernel network hardening applied
- [ ] `fail2ban` active for SSH service
- [ ] Automatic security updates configured
- [ ] `chrony` time synchronization running
- [ ] `auditd` monitoring system files
