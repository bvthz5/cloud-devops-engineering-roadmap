# 06 — Automated Security Updates, Patching, and Reboot Management

---

## 1. Enterprise Patch Management Principles

In production environments, neglecting operating system updates exposes servers to critical vulnerabilities (CVEs), remote code execution, and ransomware. However, unmonitored blind upgrades can introduce breaking changes.

**Production Patching Best Practices:**
1. **Automate Security Patches ONLY:** Configure automated tooling to install security and critical bugfix errata while holding back major feature upgrades.
2. **Staged Rollouts:** Deploy patches to Development nodes on Day 1, Staging nodes on Day 3, and Canary/Production nodes on Day 7.
3. **Automate Reboot Orchestration:** Schedule automated reboots during defined low-traffic maintenance windows.

---

## 2. Automated Updates on Ubuntu: `unattended-upgrades`

Ubuntu provides the **`unattended-upgrades`** daemon to automatically download and apply security errata silently:

```bash
# 1. Install package
sudo apt install -y unattended-upgrades update-notifier-common

# 2. Enable automated updates
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### Tuning `/etc/apt/apt.conf.d/50unattended-upgrades`
```ini
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
    // Exclude general updates to prevent unintended breaking changes:
    // "${distro_id}:${distro_codename}-updates";
};

// Automatically reboot if a kernel patch requires it:
Unattended-Upgrade::Automatic-Reboot "true";

// Specify reboot maintenance window (e.g., 03:00 AM UTC):
Unattended-Upgrade::Automatic-Reboot-Time "03:00";

// Send email notification on failure:
Unattended-Upgrade::Mail "sre-alerts@company.org";
Unattended-Upgrade::MailReport "on-change";
```

---

## 3. Automated Updates on RHEL / Rocky Linux: `dnf-automatic`

On Red Hat distributions, install and configure **`dnf-automatic`**:

```bash
# 1. Install package
sudo dnf install -y dnf-automatic

# 2. Configure /etc/dnf/automatic.conf
sudo sed -i 's/upgrade_type = default/upgrade_type = security/' /etc/dnf/automatic.conf
sudo sed -i 's/apply_updates = no/apply_updates = yes/' /etc/dnf/automatic.conf

# 3. Enable the systemd timer
sudo systemctl enable --now dnf-automatic.timer
```

---

## 4. Detecting Pending Reboots: `needrestart`

When a security patch updates a shared library (such as `libssl3` or `glibc`), running processes (like Nginx or Docker) **continue executing the old, vulnerable library in memory** until the process or host is restarted!

### 1. Check if a Full Host Reboot is Required
On Ubuntu:
```bash
if [ -f /var/run/reboot-required ]; then
    echo "CRITICAL: System reboot required to load new kernel!"
    cat /var/run/reboot-required.pkgs
fi
```

### 2. Inspect Outdated Processes with `needrestart`
The **`needrestart`** tool scans system RAM and inspects open file descriptors to determine which daemons must be restarted to apply newly patched libraries:
```bash
sudo apt install -y needrestart
sudo needrestart -b
```
- `-b`: Batch mode (returns exit code `0` if OK, `2` if services need restart, `3` if kernel reboot required).

---

## 5. Kernel Live Patching (Zero-Downtime Patching)

For high-availability clusters where rebooting nodes violates SLA requirements:
- **Canonical Livepatch (Ubuntu):** Applies kernel security fixes directly into active RAM using `kpref`.
- **AWS Kernel Live Patching (Amazon Linux 2/2023):** Patches the active Linux kernel without stopping running EC2 instances.
- **kpatch (RHEL / Rocky):** Red Hat's open-source dynamic kernel patching technology.

```bash
# Example enabling Canonical Livepatch on Ubuntu:
sudo snap install canonical-livepatch
sudo canonical-livepatch enable <YOUR_AUTH_TOKEN>
sudo canonical-livepatch status
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Compiling and Installing Software from Source](./05-Compiling-and-Installing-Software-from-Source.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
