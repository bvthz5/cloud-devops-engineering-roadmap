# 08 — Package Management Troubleshooting Guide

Production package management issues can disrupt CI/CD pipelines, block cloud VM provisioning via `cloud-init`, and cause downtime during routine patch windows. This guide details diagnosing and resolving package manager failures across Debian/Ubuntu, RHEL/Rocky, and Alpine Linux.

---

## 1. Debian/Ubuntu APT & DPKG Troubleshooting

### 1.1 "Could not get lock /var/lib/dpkg/lock-frontend"
**Symptoms:**
```bash
E: Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 1420 (unattended-upgr)
N: Be aware that removing the lock file is not a solution and may break your system.
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```

**Root Cause:**
Another process (often `unattended-upgrades`, `apt-daily.service`, or an active admin SSH session) is currently using APT/DPKG.

**Resolution Protocol:**
1. Identify the holding process:
   ```bash
   sudo lsof /var/lib/dpkg/lock-frontend
   sudo fuser /var/lib/dpkg/lock-frontend
   ```
2. Check if the process is legitimately working (e.g. system boot security patching):
   ```bash
   ps aux | grep -E 'apt|dpkg|unattended'
   ```
3. If it is an active unattended-upgrade during system start, **wait** 60–120 seconds.
4. If the process is orphaned or frozen:
   ```bash
   sudo kill -15 <PID>
   sleep 5
   # If still unresponsive:
   sudo kill -9 <PID>
   ```
5. If lock files remain stale after the process has terminated:
   ```bash
   sudo rm -f /var/lib/dpkg/lock-frontend
   sudo rm -f /var/lib/dpkg/lock
   sudo rm -f /var/cache/apt/archives/lock
   sudo dpkg --configure -a
   ```

---

### 1.2 "dpkg was interrupted, you must manually run 'sudo dpkg --configure -a'"
**Root Cause:**
A power cut, VM restart, or killed process interrupted a package installation halfway through unpacking or script execution.

**Resolution Protocol:**
```bash
# 1. Re-run pending post-installation scripts
sudo dpkg --configure -a

# 2. Fix broken dependency graph
sudo apt-get install -f -y

# 3. Clean incomplete downloaded archives
sudo apt-get clean
sudo apt-get update
```

---

### 1.3 Sub-process /usr/bin/dpkg returned an error code (1)
**Root Cause:**
A `postinst` or `prerm` maintainer script failed (e.g., service failed to start, user creation failed, or syntax error).

**Diagnostic & Resolution Protocol:**
1. Inspect the specific package script:
   ```bash
   # Maintainer scripts live in /var/lib/dpkg/info/
   ls -l /var/lib/dpkg/info/<package-name>*
   cat /var/lib/dpkg/info/<package-name>.postinst
   ```
2. Manually test running the postinst script in debug mode:
   ```bash
   sudo bash -x /var/lib/dpkg/info/<package-name>.postinst configure
   ```
3. If the script is blocked on a service that cannot start in a Docker container (e.g., systemd not running):
   - Edit `/var/lib/dpkg/info/<package-name>.postinst`
   - Add `exit 0` at the second line to bypass the non-fatal startup check
   - Run `sudo dpkg --configure -a`

---

### 1.4 GPG / Signature Errors ("The following signatures couldn't be verified")
**Symptoms:**
```text
W: GPG error: https://packages.redis.io/deb jammy InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 1234567890ABCDEF
E: The repository 'https://packages.redis.io/deb jammy InRelease' is not signed.
```

**Resolution Protocol:**
```bash
# Modern method (Ubuntu 22.04+ / Debian 12+)
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/redis-archive-keyring.gpg
sudo chmod a+r /etc/apt/keyrings/redis-archive-keyring.gpg

# Ensure sources.list entry has [signed-by=/etc/apt/keyrings/redis-archive-keyring.gpg]
sudo apt-get update
```

---

## 2. RHEL / Rocky / CentOS DNF & RPM Troubleshooting

### 2.1 RPM Database Corruption
**Symptoms:**
```text
error: rpmdb: BDB0113 Thread/process 1234 failed: BDB1507 Thread died in Berkeley DB library
error: db5 error(-30973) from dbenv->failchk: BDB0087 DB_RUNRECOVERY: Fatal error, run database recovery
error: cannot open Packages index using db5 - (-30973)
```

**Resolution Protocol:**
```bash
# 1. Terminate any stuck rpm/dnf processes
sudo killall -9 rpm dnf yum 2>/dev/null

# 2. Backup corrupted rpmdb
sudo mkdir -p /var/lib/rpm/backup
sudo cp -a /var/lib/rpm/__db* /var/lib/rpm/backup/ 2>/dev/null || true

# 3. Remove stale lock/database index files
sudo rm -f /var/lib/rpm/__db*

# 4. Rebuild the RPM database
sudo rpm --rebuilddb

# 5. Verify integrity
rpm -qa | head -n 10
sudo dnf clean all
sudo dnf check
```

---

### 2.2 DNF Dependency Conflicts & Broken Trees
**Symptoms:**
```text
Error: 
 Problem: package nginx-1:1.20.1-10.el9.x86_64 requires libcrypto.so.3(OPENSSL_3.0.0)(64bit), but none of the providers can be installed
  - cannot install the best update candidate for package...
```

**Resolution Protocol:**
```bash
# Check detailed conflict reasons with --allowerasing or --best
sudo dnf check --dependencies
sudo dnf repoquery --unsatisfied

# Check enabled repository conflicts
dnf repolist

# Run update allowing replacement of obsolete or conflicting build dependencies
sudo dnf update --nobest --allowerasing
```

---

## 3. Alpine Linux APK Troubleshooting

### 3.1 "UNTRUSTED signature" or "BAD signature"
**Symptoms:**
```text
WARNING: Ignoring https://dl-cdn.alpinelinux.org/alpine/v3.19/main: UNTRUSTED signature
```
**Resolution:**
```bash
# Re-install alpine-keys
apk update --allow-untrusted
apk add --upgrade alpine-keys --allow-untrusted
apk update
```

### 3.2 DNS Resolution Failure Inside Docker Build
**Symptoms:**
```text
fetch https://dl-cdn.alpinelinux.org/alpine/v3.19/main/x86_64/APKINDEX.tar.gz
WARNING: fetch https://...: temporary error (try again later)
ERROR: unable to select packages:
```
**Resolution:**
Specify host DNS in Docker daemon configuration (`/etc/docker/daemon.json`) or check Docker network interface:
```json
{
  "dns": ["8.8.8.8", "1.1.1.1"]
}
```

---

## 4. Production Golden Rules & Triage Runbook

```text
+-----------------------------+-------------------------------------------------------------+
| Condition                   | Safe Recovery Command                                       |
+-----------------------------+-------------------------------------------------------------+
| APT lock held by systemd    | Wait 60s, or `sudo systemctl stop apt-daily.service`        |
| DPKG interrupted unpack     | `sudo dpkg --configure -a && sudo apt-get install -f`       |
| RPM DB corrupted            | `sudo rm -f /var/lib/rpm/__db* && sudo rpm --rebuilddb`     |
| APT missing public key      | Fetch key to `/etc/apt/keyrings/` and link in repo config    |
| Space full in /var/cache    | `apt-get clean` (Debian) or `dnf clean all` (RHEL)          |
+-----------------------------+-------------------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
