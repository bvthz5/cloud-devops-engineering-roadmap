# 04 — SSH Client Configuration and Bastions

Configure `~/.ssh/config`:

```ini
Host bastion
    HostName 203.0.113.10
    User ec2-user
    Port 2222
    IdentityFile ~/.ssh/id_bastion

Host internal-web
    HostName 10.0.1.50
    User ubuntu
    ProxyJump bastion
```

Connect directly through the bastion in one step:
```bash
ssh internal-web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Hardening OpenSSH Server sshd_config](./03-Hardening-OpenSSH-Server-sshd_config.md) | [Index](../../../README.md) | [05 - SSH Tunneling and Port Forwarding Mastery →](./05-SSH-Tunneling-and-Port-Forwarding-Mastery.md) |
