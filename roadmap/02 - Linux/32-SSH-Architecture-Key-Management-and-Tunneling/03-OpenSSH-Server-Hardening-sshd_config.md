# 03 — OpenSSH Server Hardening (sshd_config)

Securing the OpenSSH daemon (`sshd`) prevents automated botnet dictionary attacks and eliminates unauthorized access.

---

## 1. Production Hardened Configuration

Edit `/etc/ssh/sshd_config.d/99-hardened.conf` (or `/etc/ssh/sshd_config`):

```ini
# Network & Port
Port 2222                        # Change default port (mitigates 99% of noisy scanners)
AddressFamily inet               # Bind to IPv4 only if IPv6 unused

# Authentication Hardening
PermitRootLogin no               # NEVER allow direct root login!
PasswordAuthentication no        # Disable passwords completely (Keys only!)
PubkeyAuthentication yes
AuthenticationMethods publickey  # Require public key
MaxAuthTries 3                   # Drop connection after 3 failed attempts
LoginGraceTime 30                # Disconnect unauthenticated clients after 30s

# Session Security
X11Forwarding no                 # Disable GUI forwarding
MaxStartups 10:30:100            # Throttles concurrent unauthenticated connections
ClientAliveInterval 300          # Send keepalive ping every 5 minutes
ClientAliveCountMax 2            # Drop inactive clients after 10 minutes

# Access Control
AllowGroups ssh-users            # Restrict SSH access to specific group members
```

---

## 2. Testing and Reloading sshd

> [!CAUTION]
> Always test configuration syntax before reloading sshd, and keep an existing SSH session open while testing!

```bash
# Validate syntax and print active parsed configuration
sudo sshd -t
sudo sshd -T

# Reload configuration
sudo systemctl reload sshd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Modern SSH Keys](./02-Modern-SSH-Key-Types-Ed25519-vs-RSA.md) | [README](./README.md) | [04 - Client Config & Bastions](./04-SSH-Client-Configuration-and-Bastion-Jump-Hosts.md) |
