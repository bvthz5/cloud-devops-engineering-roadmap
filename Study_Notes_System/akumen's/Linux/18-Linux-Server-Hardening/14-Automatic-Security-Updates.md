# 14. Automatic Security Updates

## Debian / Ubuntu (`unattended-upgrades`)

Install package:
```bash
sudo apt install unattended-upgrades -y
```

Enable automatic security updates (`/etc/apt/apt.conf.d/50unattended-upgrades`):
```ini
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
};

// Automatically reboot if required by kernel updates
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "02:00";
```

Enable daily run (`/etc/apt/apt.conf.d/20auto-upgrades`):
```ini
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

## RHEL / CentOS (`dnf-automatic`)

Install & configure:
```bash
sudo dnf install dnf-automatic -y
```

Edit `/etc/dnf/automatic.conf`:
```ini
upgrade_type = security
apply_updates = yes
```

Enable service:
```bash
sudo systemctl enable --now dnf-automatic.timer
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - File Integrity AIDE Rootkit Checkers](./13-File-Integrity-AIDE-Rootkit-Checkers.md) | [README](./README.md) | [15 - Time Sync and SELinux AppArmor](./15-Time-Sync-and-SELinux-AppArmor.md) |
