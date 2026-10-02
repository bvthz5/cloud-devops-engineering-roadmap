# 03 — YUM, DNF, and RPM Deep Dive: RHEL, CentOS, and Rocky Linux

---

## 1. The Red Hat Enterprise Linux Packaging Ecosystem

On enterprise Red Hat distributions (RHEL, CentOS Stream, Rocky Linux, AlmaLinux, Fedora), packages are distributed in the **`.rpm`** format.

```text
Evolution of Red Hat Package Managers:
RPM (1997) ──► YUM (2002) ──────────────────► DNF (2015 - Present)
Low-level      Python-based, slow             C-based (libsolv), fast,
single .rpm    dependency solver              SAT-based dependency solver,
installer      (Replaced in RHEL 8+)          modular app streams
```

---

## 2. Repository Configuration: `/etc/yum.repos.d/*.repo`

DNF queries `.repo` files located in `/etc/yum.repos.d/`. Each file contains INI-style repository definitions:

```ini
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

### Key Parameters:
- **`[id]`:** Unique identifier for the repository.
- **`baseurl`:** Direct URL to the repository mirror. Uses variables:
  - `$releasever`: Operating system major version (e.g., `9`).
  - `$basearch`: CPU architecture (e.g., `x86_64`, `aarch64`).
- **`enabled=1`:** Enables the repository during package queries.
- **`gpgcheck=1`:** Enforces cryptographic signature verification before installing any package.
- **`gpgkey`:** URL or local path to the distribution's public GPG signing key.

---

## 3. Extra Packages for Enterprise Linux (EPEL)

RHEL and Rocky Linux prioritize extreme stability, which means their default base repositories intentionally exclude many modern open-source utilities (like `htop`, `nginx`, `certbot`, `ripgrep`).

**EPEL** is an official Fedora Special Interest Group repository that compiles high-quality add-on packages for RHEL/Rocky Linux without modifying core system libraries.

```bash
# Enable EPEL on Rocky Linux / AlmaLinux / RHEL 9:
sudo dnf install -y epel-release

# Update repository metadata cache
sudo dnf makecache
```

---

## 4. DNF Modularity: Application Streams

In older enterprise Linux versions, you were locked into a single version of a programming runtime (e.g., Python 3.6 for 10 years). RHEL 8 and 9 introduced **Application Streams (Modules)**:

A single repository can provide multiple independent versions of the same software (e.g., Node.js 18, 20, and 22), allowing you to select which stream to enable:

```bash
# 1. List available module streams for Node.js
dnf module list nodejs

# Output:
# Name     Stream    Profiles    Summary
# nodejs   18        default     Javascript runtime
# nodejs   20 [d]    default     Javascript runtime (default)
# nodejs   22        default     Javascript runtime

# 2. Enable Node.js version 22 stream
sudo dnf module enable -y nodejs:22

# 3. Install Node.js from the activated stream
sudo dnf install -y nodejs
```

---

## 5. DNF History & Transaction Rollback

One of DNF's greatest superpowers for SREs is its built-in transactional history database:

```bash
# 1. View all package installation and upgrade transactions
dnf history

# Sample Output:
# ID | Command Line             | Date and time    | Action(s) | Altered
# 12 | install nginx            | 2026-10-02 08:00 | Install   | 4
# 11 | upgrade -y               | 2026-10-01 14:22 | Upgrade   | 45

# 2. Inspect exact details of transaction 12
dnf history info 12

# 3. ROLLBACK a broken installation or upgrade cleanly!
sudo dnf history undo 12
```

---

## 6. Low-Level RPM Command Reference

```bash
# 1. Query if a package is installed
rpm -q nginx

# 2. List all installed packages on the operating system
rpm -qa | grep -i python

# 3. List all files installed on disk by an RPM package
rpm -ql nginx

# 4. Find which package owns a specific file
rpm -qf /usr/sbin/nginx

# 5. Verify file integrity (Check if files have been modified or corrupted)
rpm -V nginx
# Output flags:
# S = File size differs
# M = Mode/permissions differ
# 5 = MD5/SHA256 checksum differs (file content altered!)
# T = Modification time differs

# 6. Install a local RPM package with verbose progress bar
sudo rpm -ivh package.rpm
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - APT and DPKG Deep Dive Debian Ubuntu](./02-APT-and-DPKG-Deep-Dive-Debian-Ubuntu.md) | [README](./README.md) | [04 - APK Package Manager Alpine Linux and Containers](./04-APK-Package-Manager-Alpine-Linux-and-Containers.md) |
