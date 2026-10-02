# 18 - Troubleshooting & Diagnostic Playbooks

Systematic playbooks for resolving common file management errors and terminal blockages.

---

## 🚨 Incident 1: "Argument list too long"

### The Symptom:
You navigate to a directory containing 500,000 files and attempt to clean it:
```bash
$ rm *
bash: /usr/bin/rm: Argument list too long
```

### The Root Cause:
The Linux kernel limits the maximum length of command-line arguments (`ARG_MAX`) passed via `execve()`. When the shell expands `*` into 500,000 individual filenames, the resulting command string exceeds the kernel argument buffer size.

### Resolution:
Bypass shell wildcard expansion using `find`:
```bash
# Efficient in-kernel deletion without shell wildcard expansion:
$ find . -type f -delete

# Alternative using xargs:
$ find . -type f -print0 | xargs -0 rm -f
```

---

## 🚨 Incident 2: "Permission denied" or "Operation not permitted"

### The Symptom:
A developer executes a script:
```bash
$ ./deploy.sh
bash: ./deploy.sh: Permission denied
```

### Diagnostic & Resolution Step-by-Step:

```bash
# 1. Check file permissions:
$ ls -l deploy.sh
-rw-r--r-- 1 ubuntu ubuntu 450 Sep 13 10:00 deploy.sh

# Notice: Missing execute (+x) permission!
$ chmod +x deploy.sh

# 2. If 'sudo' still yields "Operation not permitted":
# Check if the file has the immutable attribute set (+i):
$ lsattr deploy.sh
----i---------e---- deploy.sh

# Remove immutable attribute using chattr:
$ sudo chattr -i deploy.sh
```

---

## 🚨 Incident 3: `/bin/bash^M: bad interpreter: No such file or directory`

### The Symptom:
A shell script written on Windows fails immediately when transferred to a Linux server:
```bash
$ ./script.sh
bash: ./script.sh: /bin/bash^M: bad interpreter: No such file or directory
```

### The Root Cause:
Windows uses **CRLF (`\r\n`)** line endings, while Linux uses **LF (`\n`)**. The extra hidden carriage return (`\r`, displayed as `^M`) corrupts the shebang line (`#!/bin/bash\r`).

### Resolution:
```bash
# 1. Convert CRLF to LF using dos2unix:
$ dos2unix script.sh

# 2. Or strip carriage returns using sed in-place:
$ sed -i 's/\r$//' script.sh
```

---

## 🚨 Incident 4: "Device or resource busy"

### The Symptom:
Attempting to unmount a filesystem yields:
```bash
$ sudo umount /mnt/storage
umount: /mnt/storage: target is busy.
```

### Diagnostic & Resolution:
```bash
# 1. Identify which process or shell is currently inside the directory:
$ sudo lsof +D /mnt/storage
# OR:
$ sudo fuser -v /mnt/storage

# 2. Kill the blocking process:
$ sudo fuser -k -m /mnt/storage
$ sudo umount /mnt/storage
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Real World Scenarios](./17-Real-World-Scenarios.md) | [README](./README.md) | [19 - Interview QA](./19-Interview-QA.md) |
