# 03 - User Creation and Management

Linux provides two utilities for creating user accounts: `useradd` (low-level utility across all Linux distributions) and `adduser` (high-level interactive Perl script found primarily in Debian/Ubuntu).

---

## ⚔️ `useradd` vs. `adduser` Comparison

| Feature | `useradd` | `adduser` |
| :--- | :--- | :--- |
| **Tool Type** | Low-level binary executable binary (`/usr/sbin/useradd`) | High-level interactive Perl wrapper script (`/usr/sbin/adduser`) |
| **Availability** | Available on all Linux distributions (RHEL, Debian, Arch, Alpine, SLES) | Native to Debian/Ubuntu (Requires installation on RHEL/Fedora) |
| **Default Behavior** | Does **NOT** create home directory or set password unless flags are specified (on some distros) | Interactively prompts for password, full name, room number, and automatically populates `/home/username` |
| **Scripting Suitability** | **Ideal for automation**, CI/CD pipelines, and shell scripts | Designed for interactive manual terminal user creation |

---

## 🛠️ Using `useradd` (Low-Level / Automated)

### Common Syntax & Useful Flags

```bash
useradd [options] username
```

| Option Flag | Long Flag | Purpose & Action |
| :--- | :--- | :--- |
| `-m` | `--create-home` | Automatically creates the user home directory (`/home/username`). |
| `-d /path` | `--home-dir` | Specifies a custom path for home directory. |
| `-s /shell` | `--shell` | Sets default login shell (e.g., `-s /bin/bash` or `-s /sbin/nologin`). |
| `-u UID` | `--uid` | Sets explicit numeric User ID. |
| `-g Group/GID` | `--gid` | Sets user's **primary group** name or numeric GID. |
| `-G group1,group2` | `--groups` | Sets list of **supplementary (secondary) groups**. |
| `-c "Comment"` | `--comment` | Sets GECOS text description (e.g., user full name or team). |
| `-e YYYY-MM-DD` | `--expiredate` | Sets account expiration date. |
| `-r` | `--system` | Creates a system user (UID < 1000, no home directory by default). |

### Practical Examples

1. **Create standard user with home directory and bash shell:**
   ```bash
   sudo useradd -m -s /bin/bash alice
   ```

2. **Create user with custom UID, primary group, supplementary groups, and comment:**
   ```bash
   sudo useradd -m -u 1050 -g developers -G sudo,docker -c "Alice Smith - Lead DevOps" -s /bin/bash alice
   ```

3. **Create unprivileged system account for background daemon (e.g. Prometheus exporter):**
   ```bash
   sudo useradd -r -s /sbin/nologin -d /var/lib/prometheus prometheus
   ```

---

## ⚙️ Configuration Files for Default User Creation Settings

When `useradd` is run without specific flags, defaults are pulled from two key system templates:

### 1. `/etc/default/useradd`
Defines default parameters used during execution:
```text
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```
* To view default configuration: `useradd -D`
* To modify default shell system-wide: `sudo useradd -D -s /bin/zsh`

### 2. Skeleton Directory (`/etc/skel`)
Whenever `useradd -m` creates a new home directory, files inside `/etc/skel` (e.g., `.bashrc`, `.bash_logout`, `.profile`) are copied into the new user's home folder with correct file ownership.

```text
/etc/skel/
├── .bash_logout
├── .bashrc
└── .profile
```

> **DevOps Tip:** Customize `/etc/skel/.bashrc` or add standard SSH config directories (`/etc/skel/.ssh/authorized_keys`) in custom golden images so every newly created user gets standardized aliases and environments!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Core Configuration Files](./02-Core-Configuration-Files.md) | [README](./README.md) | [04 - Password Management and Aging](./04-Password-Management-and-Aging.md) |
