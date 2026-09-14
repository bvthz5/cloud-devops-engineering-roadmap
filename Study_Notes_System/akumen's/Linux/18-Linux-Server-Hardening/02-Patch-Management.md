# 2. Package and Patch Management

## Why Patching Matters
Unpatched software vulnerabilities (CVEs) are the leading vector for automated server exploits. Regular update maintenance ensures security patches are applied promptly.

## Package Update Commands

### Debian / Ubuntu (`apt`)
```bash
# Update package index
sudo apt update

# Upgrade all installed packages with security updates
sudo apt upgrade -y

# Perform distribution upgrade (handles dependency changes)
sudo apt full-upgrade -y

# Clean unused packages
sudo apt autoremove -y
```

### RHEL / CentOS / Rocky Linux (`dnf` / `yum`)
```bash
# Check available security updates
sudo dnf updateinfo list security

# Apply only security patches
sudo dnf update --security -y

# Upgrade all packages
sudo dnf upgrade -y
```

## Patch Management Best Practices
1. **Subscribe to Security Advisories:** Monitor Ubuntu Security Notices (USN) or Red Hat Security Advisories (RHSA).
2. **Staging Environment First:** Always test updates on non-production systems before deploying to production.
3. **Automate Security Patches:** Use `unattended-upgrades` or `dnf-automatic` for critical security errata.
