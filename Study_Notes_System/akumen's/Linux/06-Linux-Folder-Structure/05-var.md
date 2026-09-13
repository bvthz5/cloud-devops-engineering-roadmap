# 05 - The `/var` Directory (Variable Data)

The `/var` directory stores **Variable Data**—files whose size and content continuously change during standard operating system runtime. This includes log files, database storage, email spools, transient caches, and web content.

---

## 📂 Key Subdirectories in `/var`

```text
/var/
├── log/          # System, kernel, and service log files (syslog, auth.log, nginx/)
├── lib/          # Persistent application state & database files (mysql/, docker/, dpkg/)
├── spool/        # Queued data awaiting processing (cron, mail, print jobs)
├── cache/        # Application cache data (apt cache, man page cache)
├── back-ups/     # Service backup archives
└── www/          # Default document root directory for web servers (Nginx, Apache)
```

---

## 🔍 In-Depth Subdirectory Breakdown

### 1. `/var/log/`
The central hub for system logging and observability:
- `/var/log/syslog` or `/var/log/messages`: General operating system events.
- `/var/log/auth.log` or `/var/log/secure`: SSH logins, sudo usage, and security events.
- `/var/log/nginx/access.log`: HTTP web access logs.
- `/var/log/journal/`: Systemd binary journal data.

### 2. `/var/lib/`
Persistent operational state data for packages and databases:
- `/var/lib/mysql/` or `/var/lib/postgresql/`: Database table data blocks.
- `/var/lib/docker/`: Container images, layers, volumes, and runtime containers.
- `/var/lib/dpkg/` or `/var/lib/rpm/`: Package manager state tracking installed software packages.

### 3. `/var/spool/`
Holds data queued for upcoming background execution, such as outgoing email queues (`/var/spool/mail`) or cron job spools (`/var/spool/cron/crontabs`).

---

## ⚠️ Disk Exhaustion Risk in `/var`

Because logs and database writes continuously expand `/var`, runaway processes or unrotated logs can consume 100% of disk space.

```bash
# Check large directory sizes under /var
sudo du -sh /var/* | sort -hr | head -n 10

# Configure log rotation rules to compress and delete old logs automatically
cat /etc/logrotate.conf
```

In production architectures, `/var` (or `/var/log`) is often mounted on a **separate disk volume** so log exhaustion does not freeze the root partition (`/`).

---

## ⬅️ Navigation
- Previous: [04 - `/etc` Directory](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/04-etc.md)
- Next: [06 - `/home` and `/root`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/06-home-and-root.md)
