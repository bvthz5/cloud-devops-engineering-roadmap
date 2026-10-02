# 04 — SSH Client Configuration and Bastion Jump Hosts

Managing dozens of cloud servers across development, staging, and production VPCs is streamlined using `~/.ssh/config`.

---

## 1. Configuration Syntax: `~/.ssh/config`

```ini
# Global defaults for all hosts
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519

# Public Cloud Bastion / Jumpbox Host
Host bastion
    HostName 203.0.113.10
    User ec2-user
    Port 2222
    IdentityFile ~/.ssh/id_bastion_ed25519

# Private Production Web Server behind Bastion
Host prod-web
    HostName 10.0.1.50
    User ubuntu
    ProxyJump bastion
```

Now, connecting to the private VPC host requires only:
```bash
ssh prod-web
```
SSH automatically connects to the bastion in the background and transparently proxies traffic to `10.0.1.50`!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Server Hardening](./03-OpenSSH-Server-Hardening-sshd_config.md) | [README](./README.md) | [05 - SSH Tunneling](./05-SSH-Tunneling-and-Port-Forwarding.md) |
