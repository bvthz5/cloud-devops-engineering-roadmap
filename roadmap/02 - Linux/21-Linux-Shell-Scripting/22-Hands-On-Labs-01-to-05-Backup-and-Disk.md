# 22 — Hands-On Labs 01 to 05: Backup and Disk Management

## Lab 01: Building an Incremental `rsync` Backup Pipeline

### 1. Prerequisites & Topology
- Local Linux VM (Source: `/var/www/html`)
- Remote Linux VM (Backup Server IP: `192.168.1.150`, Destination: `/mnt/backups/`)

### 2. Step-by-Step Instructions
1. **Generate SSH Keypair on Source Machine**:
   ```bash
   ssh-keygen -t ed25519 -C "backup-key" -f ~/.ssh/id_ed25519_backup -N ""
   ```
2. **Copy Public Key to Backup Server**:
   ```bash
   ssh-copy-id -i ~/.ssh/id_ed25519_backup.pub backupuser@192.168.1.150
   ```
3. **Write Script (`lab01_rsync.sh`)**:
   ```bash
   cat <<'EOF' > lab01_rsync.sh
   #!/usr/bin/env bash
   set -euo pipefail
   rsync -avz --delete -e "ssh -i ~/.ssh/id_ed25519_backup" /var/www/html/ backupuser@192.168.1.150:/mnt/backups/html/
   EOF
   chmod +x lab01_rsync.sh
   ```
4. **Execute and Verify Output**:
   ```bash
   ./lab01_rsync.sh
   ```

---

## Lab 02: Automated Ubuntu Package Maintenance Suite

### 1. Step-by-Step Setup
1. Create script file `/usr/local/bin/apt_maintenance.sh`.
2. Insert strict mode `set -euo pipefail` and `export DEBIAN_FRONTEND=noninteractive`.
3. Add `apt-get update && apt-get upgrade -y && apt-get autoremove -y && apt-get autoclean`.
4. Test run with `sudo /usr/local/bin/apt_maintenance.sh`.

---

## Lab 03: Disk Space Capacity Alert System

### 1. Step-by-Step Setup
1. Create test loop partition using `dd` and `mkfs.ext4` to simulate full disk (90%+ usage).
2. Write `df -hP` parsing script using `awk`.
3. Verify that script triggers exit status `2` when threshold is exceeded.

---

## Lab 04: Log Cleanup Retention Policy Execution

### 1. Step-by-Step Setup
1. Create test log files with backdated modification times (`touch -d "20 days ago" /tmp/test_logs/old.log`).
2. Run log cleanup script searching for `-mtime +14`.
3. Confirm that backdated files are deleted while recent log files remain intact.

---

## Lab 05: Bulk User Creation from CSV Matrix

### 1. Step-by-Step Setup
1. Create test CSV `users.csv`:
   ```csv
   username,group,shell
   dev_alice,developers,/bin/bash
   dev_bob,developers,/bin/bash
   sys_charlie,devops,/bin/zsh
   ```
2. Run `sudo ./05_bulk_user_creation.sh users.csv`.
3. Verify created users in `/etc/passwd` using `getent passwd dev_alice`.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [21 - DevOps Case Studies 16-20](./21-DevOps-Case-Studies-16-to-20.md) | [README](./README.md) | [23 - Hands-On Labs 06-10](./23-Hands-On-Labs-06-to-10-Log-and-Security.md) |
