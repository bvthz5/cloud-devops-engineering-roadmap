# 04 - The `/etc` Directory (Editable Text Configuration)

The `/etc` directory (historically *et cetera*, now commonly referenced as **Editable Text Configuration**) stores system-wide configuration files and initialization scripts required by operating system services and installed applications.

---

## 📌 Rules & Properties of `/etc`

- **Static Text Files:** `/etc` contains human-readable plain text configuration files (YAML, JSON, INI, Conf, XML).
- **No Executable Binaries:** Binaries are stored in `/bin`, `/usr/bin`, or `/sbin`. `/etc` contains configuration rules only.
- **Host-Specific:** Configuration in `/etc` applies strictly to the local machine instance.

---

## 📂 Crucial Configuration Files in `/etc`

| Configuration File / Directory | Purpose & Description |
| :--- | :--- |
| `/etc/fstab` | Static filesystem mount table defining how partitions, remote NFS shares, and swap volumes are attached at boot. |
| `/etc/passwd` | User account metadata (username, UID, GID, home directory, default shell). |
| `/etc/shadow` | Secure, encrypted user password hashes (accessible only by root/shadow group). |
| `/etc/group` | Defines local system groups and group member lists. |
| `/etc/sudoers` | Access control rules for `sudo` administrative privilege delegation. |
| `/etc/hosts` | Static domain name to IP address mapping lookup table. |
| `/etc/resolv.conf` | DNS name server IP addresses used by host resolver. |
| `/etc/hostname` | Local system hostname identifier. |
| `/etc/systemd/system/` | Systemd unit configuration files for services, sockets, and targets managed by `systemctl`. |
| `/etc/nginx/` / `/etc/apache2/` | Web server configuration directives and virtual host declarations. |
| `/etc/ssh/sshd_config` | SSH daemon security rules, port bindings, and authentication settings. |
| `/etc/crontab` & `/etc/cron.*/` | System-wide automated scheduled tasks (cron jobs). |

---

## 🛠️ DevOps Best Practice: Versioning `/etc` with `etckeeper`

Because unexpected edits to `/etc` files can crash services or lock administrators out of SSH, DevOps engineers manage `/etc` using Git version control tools like `etckeeper`:

```bash
# Install etckeeper to automatically track /etc edits in git
sudo apt install etckeeper

# Check status of configuration changes in /etc
cd /etc && sudo git status
```

---

## ⬅️ Navigation
- Previous: [03 - `/usr` Directory](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/03-usr.md)
- Next: [05 - `/var` Directory](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/05-var.md)
