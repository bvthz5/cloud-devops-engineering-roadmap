# 01 - User Management Fundamentals

User management is a primary mechanism for security, process isolation, access control, and accounting in Linux operating systems. 

---

## 🔑 Core Concepts: UIDs, GIDs, and Account Types

The Linux kernel does not track processes or permissions by string usernames (like `alice` or `nginx`). Internally, the kernel processes everything via **Numeric Identifiers**:
* **UID (User Identifier):** A unique numerical integer assigned to every user account on the system.
* **GID (Group Identifier):** A unique numerical integer assigned to every group on the system.

### UID Allocation Ranges (Standard FHS / Systemd Conventions)

| UID Range | Account Type | Description & Examples |
| :--- | :--- | :--- |
| **`0`** | **Superuser / Root** | Has unrestricted administrative privileges across the entire operating system kernel. |
| **`1 - 999`** *(RHEL/CentOS)*<br>**`1 - 99`** *(Older Debian)* | **System Accounts** | Reserved for background services, daemons, and system applications (e.g., `bin`, `daemon`, `sys`, `sshd`, `nginx`, `postgres`, `systemd-nobody`). They typically do not have an interactive login shell (`/sbin/nologin` or `/bin/false`). |
| **`1000 - 60000`** *(Debian/Ubuntu/RHEL default)* | **Normal / Human Users** | Assigned to regular interactive human users or software developers logging into the machine via SSH or console terminal. |
| **`65534`** | **Nobody / Overflow** | Unprivileged user used by NFS (Network File System) and background tasks requiring minimal permissions (`nobody`/`nogroup`). |

---

## 👥 Primary vs. Supplementary Groups

Every user account on Linux belongs to **at least one Primary Group** and can optionally belong to **zero or more Supplementary (Secondary) Groups**.

```text
               ┌──────────────────────────────────────────────┐
               │              User Account: alice             │
               │                  (UID: 1001)                 │
               └──────────────────────┬───────────────────────┘
                                      │
              ┌───────────────────────┴───────────────────────┐
              │                                               │
              ▼                                               ▼
  ┌──────────────────────┐                       ┌─────────────────────────┐
  │    Primary Group     │                       │  Supplementary Groups   │
  │     alice (GID 1001) │                       │  docker (GID 998)       │
  ├──────────────────────┤                       │  sudo   (GID 27)        │
  │ Files created by     │                       │  devs   (GID 1005)      │
  │ default inherit this │                       ├─────────────────────────┤
  │ group ownership.     │                       │ Grant additional access │
  └──────────────────────┘                       │ permissions to shared   │
                                                 │ directories & binaries. │
                                                 └─────────────────────────┘
```

1. **Primary Group:**
   * Specified in `/etc/passwd` (the 4th field).
   * When a user creates a new file or directory, the file's group ownership is automatically set to the user's primary group (unless SGID bit on parent folder overrides it).
   * Traditionally, modern Linux distros create a **User Private Group (UPG)** where each user gets a dedicated group matching their username (e.g., user `alice` has primary group `alice`).

2. **Supplementary Groups:**
   * Listed in `/etc/group` (the 4th field).
   * Grants permissions to additional resources (e.g., adding user `alice` to the `docker` supplementary group allows her to access the Docker daemon socket `/var/run/docker.sock`).

---

## ⚡ System Accounts vs. Interactive Accounts

| Feature | System Accounts | Normal / Interactive Accounts |
| :--- | :--- | :--- |
| **Primary Purpose** | Run background services/daemons in isolated contexts | Human user login & execution |
| **Default Shell** | `/sbin/nologin` or `/bin/false` | `/bin/bash`, `/bin/zsh`, `/bin/sh` |
| **Home Directory** | Non-existent or system runtime dir (e.g., `/nonexistent`, `/var/lib/nginx`) | Standard user dir (`/home/username`) |
| **Password** | Locked / Excluded (`!`, `*` in `/etc/shadow`) | Hashed password set via `passwd` |
| **Interactive SSH Login** | Disabled | Enabled (via password or public key) |

---

## 🧠 Mental Model: The Process Credential Array

When a process is spawned by the kernel, it inherits a credential structure containing:
* **Real UID (RUID) & Real GID (RGID):** Identifies who launched the process.
* **Effective UID (EUID) & Effective GID (EGID):** Determines actual access permissions during file system system calls (evaluated for SUID/SGID binaries and `sudo`).
* **Supplementary Group List:** Array of GIDs representing all secondary groups the user belongs to.

To view current logged-in user credentials, run:
```bash
id
# Output: uid=1000(alice) gid=1000(alice) groups=1000(alice),27(sudo),998(docker)
```
