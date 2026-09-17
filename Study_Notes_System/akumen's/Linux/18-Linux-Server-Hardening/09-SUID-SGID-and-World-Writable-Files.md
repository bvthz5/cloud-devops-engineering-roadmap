# 9. SUID, SGID, and World-Writable Files

## Security Risks
- **SUID (Set User ID):** Executable runs with the permissions of the file owner (often `root`). If vulnerable, it leads to immediate Privilege Escalation.
- **World-Writable Files:** Any user can modify or overwrite file contents.

## Auditing & Remediation

### 1. Find all SUID binaries
```bash
sudo find / -xdev -type f -perm -4000 -exec ls -l {} ;
```

### 2. Remove SUID bit from non-essential binaries
```bash
sudo chmod u-s /usr/bin/newgrp
```

### 3. Find all SGID binaries
```bash
sudo find / -xdev -type f -perm -2000 -exec ls -l {} ;
```

### 4. Find all World-Writable files
```bash
sudo find / -xdev -type f -perm -0002 -exec ls -l {} ;
```

### 5. Remove World-Writable permission
```bash
sudo chmod o-w /path/to/file
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - File Permissions and Umask](./08-File-Permissions-and-Umask.md) | [README](./README.md) | [10 - Services and Port Hardening](./10-Services-and-Port-Hardening.md) |
