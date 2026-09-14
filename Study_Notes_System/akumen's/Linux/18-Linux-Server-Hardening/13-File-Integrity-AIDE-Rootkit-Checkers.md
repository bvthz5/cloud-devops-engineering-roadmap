# 13. File Integrity Monitoring & Rootkit Checkers

## 1. Advanced Intrusion Detection Environment (AIDE)
AIDE creates a cryptographic baseline hash of system files and alerts on unauthorized changes.

```bash
# Install AIDE
sudo apt install aide -y

# Initialize baseline database
sudo aideinit

# Move baseline database into production path
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# Run integrity check manually
sudo aide --check
```

## 2. Rootkit Scanners (`rkhunter` & `chkrootkit`)
Scans for hidden rootkits, backdoors, and promiscuous network interfaces.

```bash
# Install rootkit checkers
sudo apt install rkhunter chkrootkit -y

# Update rkhunter definitions database
sudo rkhunter --propupd

# Run system check
sudo rkhunter --check --sk
sudo chkrootkit
```
