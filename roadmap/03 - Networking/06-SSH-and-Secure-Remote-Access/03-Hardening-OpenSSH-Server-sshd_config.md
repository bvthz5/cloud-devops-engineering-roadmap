# 03 — Hardening OpenSSH Server (sshd_config)

Edit `/etc/ssh/sshd_config`:

```ini
# Port and Protocol
Port 2222
AddressFamily inet

# Authentication Hardening
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
LoginGraceTime 30

# Disable unused features
X11Forwarding no
```

Validate and reload:
```bash
sudo sshd -t && sudo systemctl reload sshd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Modern SSH Keys](./02-Modern-SSH-Keys-Ed25519-vs-RSA.md) | [README](./README.md) | [04 - Client Config & Bastions](./04-SSH-Client-Configuration-and-Bastions.md) |
