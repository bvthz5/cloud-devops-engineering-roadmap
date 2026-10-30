# 12 — Quick Revision Cheat Sheet: Linux Package Management

A high-density reference sheet comparing commands, file paths, and key concepts across major Linux distributions.

---

## 1. Cross-Distribution Command Translation Matrix

| Task | Debian / Ubuntu (`apt` / `dpkg`) | RHEL / Rocky / CentOS (`dnf` / `rpm`) | Alpine Linux (`apk`) |
| :--- | :--- | :--- | :--- |
| **Update repository cache** | `sudo apt update` | `sudo dnf check-update` | `apk update` |
| **Upgrade all packages** | `sudo apt upgrade -y` | `sudo dnf upgrade -y` | `apk upgrade` |
| **Install package** | `sudo apt install -y pkg` | `sudo dnf install -y pkg` | `apk add pkg` |
| **Install without caching** | `apt install -y --no-install-recommends` | `dnf install -y --setopt=keepcache=0` | `apk add --no-cache pkg` |
| **Remove package (keep conf)**| `sudo apt remove pkg` | `sudo dnf remove pkg` | `apk del pkg` |
| **Purge package + conf** | `sudo apt purge pkg` | `sudo dnf remove pkg` | `apk del --purge pkg` |
| **Search remote repo** | `apt search keyword` | `dnf search keyword` | `apk search keyword` |
| **Show package details** | `apt show pkg` | `dnf info pkg` | `apk info -a pkg` |
| **Find file provider (uninstalled)**| `apt-file search /path/file` | `dnf provides /path/file` | `apk info --who-owns /path/file` |
| **Find file owner (installed)**| `dpkg -S /path/file` | `rpm -qf /path/file` | `apk info -W /path/file` |
| **List package installed files**| `dpkg -L pkg` | `rpm -ql pkg` | `apk info -L pkg` |
| **List all installed packages**| `dpkg -l` | `rpm -qa` | `apk info` |
| **Lock / Pin package version**| `sudo apt-mark hold pkg` | `sudo dnf versionlock add pkg` | `apk add pkg=1.2.3-r0` |
| **Clean local cache** | `sudo apt clean` | `sudo dnf clean all` | `apk cache clean` |
| **Undo last transaction** | *(Manual rollback)* | `sudo dnf history undo last` | *(Manual rollback)* |

---

## 2. Key File Paths & Directories

### Debian / Ubuntu:
- `/etc/apt/sources.list` & `/etc/apt/sources.list.d/`: Repository configuration files.
- `/etc/apt/keyrings/`: Secure isolated GPG keyring storage for third-party repos.
- `/var/lib/dpkg/status`: Complete database of currently installed Debian packages.
- `/var/lib/dpkg/info/`: Maintainer scripts (`.postinst`, `.prerm`, `.md5sums`).
- `/var/cache/apt/archives/`: Cached downloaded `.deb` files.
- `/var/log/dpkg.log` & `/var/log/apt/history.log`: Installation audit trails.

### RHEL / Rocky / Fedora:
- `/etc/yum.repos.d/*.repo`: Repository definitions.
- `/var/lib/rpm/`: RPM SQLite / Berkeley database files.
- `/var/cache/dnf/`: Cached metadata and RPM packages.
- `/var/log/dnf.log`: Complete DNF transaction log.

### Alpine Linux:
- `/etc/apk/repositories`: Active Alpine package mirrors.
- `/etc/apk/keys/`: Official Alpine distribution public RSA keys.
- `/lib/apk/db/installed`: Plaintext flat-file database of all installed packages.

---

## 3. High-Priority Commands to Memorize

```bash
# Fix broken packages
sudo dpkg --configure -a && sudo apt-get install -f -y

# Rebuild corrupted RPM database
sudo rm -f /var/lib/rpm/__db* && sudo rpm --rebuilddb

# Find which package owns a specific shared library
dpkg -S libssl.so.3       # Debian/Ubuntu
rpm -qf /usr/lib64/libssl.so.3  # RHEL/Rocky

# Download package without installing it
apt-get download nginx    # Downloads .deb to current directory
dnf download nginx        # Downloads .rpm to current directory

# Inspect contents of package archive before installation
dpkg-deb -c package.deb
rpm -qpl package.rpm
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (25-Linux-Networking-and-DNS-Troubleshooting) →](../25-Linux-Networking-and-DNS-Troubleshooting/01-Linux-Network-Stack-and-Device-Model.md) |
