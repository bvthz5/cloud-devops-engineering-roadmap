# 5. SSH Hardening

SSH is the primary gateway into a remote Linux server. Hardening SSH is critical.

## Configuration File: `/etc/ssh/sshd_config`

### Key Security Settings
```ini
# Change default SSH port (mitigates automated bots)
Port 2222

# Protocol version
Protocol 2

# Disable root SSH login completely
PermitRootLogin no

# Disable password authentication (Force SSH keys)
PasswordAuthentication no
PubkeyAuthentication yes

# Disable empty passwords
PermitEmptyPasswords no

# Limit login attempts
MaxAuthTries 3

# Idle session timeout (disconnect after 5 minutes of inactivity)
ClientAliveInterval 300
ClientAliveCountMax 0

# Disable X11 and Agent forwarding unless explicitly required
X11Forwarding no
AllowAgentForwarding no

# Restrict SSH access to specific users/groups
AllowUsers devops_admin alice
AllowGroups sysadmins
```

## Restarting SSH Safely
WARNING: Keep your existing SSH session OPEN in one terminal tab while testing configuration changes in another tab!

```bash
# Validate configuration syntax first
sudo sshd -t

# Restart SSH service
sudo systemctl restart ssh
```
