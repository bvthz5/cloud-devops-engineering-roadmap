# 15. Time Synchronization & Mandatory Access Control

## 1. Network Time Protocol (Chrony)
Accurate system timestamps are required for cryptographic verification and log forensics.

```bash
# Install chrony
sudo apt install chrony -y

# Enable and start chrony service
sudo systemctl enable --now chrony

# Verify synchronization status
chronyc tracking
chronyc sources
```

## 2. Mandatory Access Control (MAC)

### SELinux (RHEL / CentOS / Fedora)
Controls processes based on security contexts.
- **Enforcing:** Active protection (Default/Recommended).
- **Permissive:** Logs violations without blocking.
- **Disabled:** Protection off.

Check status:
```bash
sestatus
```

Set to enforcing mode:
```bash
sudo setenforce 1
```

### AppArmor (Ubuntu / Debian)
Restricts individual program capabilities.

Check status:
```bash
sudo aa-status
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Automatic Security Updates](./14-Automatic-Security-Updates.md) | [README](./README.md) | [16 - Hands On Hardening Labs](./16-Hands-On-Hardening-Labs.md) |
