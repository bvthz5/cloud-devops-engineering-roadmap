# 09 - Troubleshooting & Incident Response Playbooks

---

## 🚨 Incident 1: "No Space Left on Device" But `df -h` Shows Plenty of Gigabytes!

### The Symptom:
A developer runs `touch /var/log/test.log` and receives:
```
touch: cannot touch '/var/log/test.log': No space left on device
```
However, running `df -h` shows:
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   18G   32G  36% /
```
Over 30 GB of storage is free, yet Linux refuses to create any new file.

### The Root Cause: Inode Exhaustion (`df -i`)
Filesystems allocate a fixed table of **inodes** when formatted (especially on ext4). Every file, folder, and symlink consumes **1 inode**, regardless of whether its content is 0 bytes or 10 GB. If an application creates millions of tiny session or queue files, **you run out of inodes before running out of disk blocks**.

### Step-by-Step Diagnostic & Resolution:
```bash
# 1. Inspect inode utilization:
$ df -i
Filesystem      Inodes   IUsed   IFree IUse% Mounted on
/dev/sda1      3276800 3276800       0  100% /   <-- 100% Inodes consumed!

# 2. Pinpoint which directory contains millions of tiny files:
$ sudo find /var -xdev -printf '%h\n' | sort | uniq -c | sort -rn | head -n 10
2894102 /var/spool/postfix/maildrop
  15234 /var/lib/php/sessions

# 3. Why 'rm *' fails here:
# Running 'rm /var/spool/postfix/maildrop/*' will fail with "Argument list too long".

# 4. Correct way to purge millions of tiny files fast:
$ sudo find /var/spool/postfix/maildrop/ -type f -delete
```

---

## 🚨 Incident 2: Deleted a 50 GB Log File, But `df -h` Still Shows 100% Full!

### The Symptom:
A developer notices `/var/log/app.log` has grown to 50 GB. They run:
```bash
$ sudo rm /var/log/app.log
```
Then they check `df -h`:
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        60G   60G     0 100% /
```
The disk is still 100% full! Where did the 50 GB go?

### The Root Cause: Unlinked Open File Descriptors
In Linux, running `rm` does **not** physically erase data blocks immediately. It merely deletes the directory entry (it unlinks the filename from the inode).
If a running process (e.g. Java, Nginx, Python) **still holds that file open**, the kernel keeps the inode and all disk blocks allocated until that process terminates or closes the file!

### Step-by-Step Diagnostic & Resolution:
```bash
# 1. Find processes holding open references to deleted files:
$ sudo lsof +L1
# OR
$ sudo lsof | grep deleted
java  14205  appuser  4w  REG  8,1  53687091200  0  131092 /var/log/app.log (deleted)

# Look at PID: 14205, File Descriptor: 4w, Size: 53.6 GB!

# 2. Fix Option A: Gracefully reload or restart the application:
$ sudo systemctl reload myapp

# 3. Fix Option B (Zero Downtime / Cannot restart app):
# Truncate the file descriptor to 0 bytes directly through the /proc virtual filesystem:
$ sudo truncate -s 0 /proc/14205/fd/4
# OR
$ sudo bash -c '> /proc/14205/fd/4'

# 4. Verify disk space is immediately reclaimed:
$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        60G   10G   50G  17% /
```

> [!TIP]
> **Prevention Rule:** Never use `rm` on active log files written to by live services. Instead, truncate them in place:
> ```bash
> # Safe log zeroing without restarting processes:
> $ sudo truncate -s 0 /var/log/app.log
> # OR
> $ : > /var/log/app.log
> ```

---

## 🚨 Incident 3: Read-Only Filesystem (`Read-only file system`)

### The Symptom:
Any write operation yields:
```
bash: /var/log/test: Read-only file system
```

### The Root Cause:
When the Linux kernel detects hardware I/O timeouts, disk controller errors, or filesystem corruption, its default safety action is to remount the filesystem as **Read-Only (`ro`)** to prevent cascading data destruction.

### Diagnostic & Resolution:
```bash
# 1. Inspect kernel ring buffer for disk hardware / filesystem errors:
$ sudo dmesg -T | grep -E "EXT4|XFS|error|I/O|Buffer I/O"

# 2. Check mount status:
$ mount | grep " / "
/dev/sda1 on / type ext4 (ro,relatime)   <-- Note the 'ro' flag

# 3. If disk is healthy and was triggered by a minor glitch, remount back to rw:
$ sudo mount -o remount,rw /

# 4. If filesystem corruption exists, boot into rescue mode or unmount to run fsck:
$ sudo umount /dev/sdb1
$ sudo fsck -y /dev/sdb1
```

---

## 🚨 Incident 4: Server Fails to Boot into OS After Editing `/etc/fstab`

### The Symptom:
A server reboots and halts with:
```
You are in emergency mode. After logging in, type "journalctl -xb" to view system logs...
Give root password for maintenance:
```

### The Root Cause:
A typo in `/etc/fstab` (wrong UUID, misspelled mount point, invalid filesystem type) or a missing network drive without the `nofail` option.

### Rescue Steps:
```bash
# 1. Enter root password to access emergency shell.

# 2. Notice that root filesystem is mounted read-only in emergency mode!
# Remount root as read-write:
$ mount -o remount,rw /

# 3. Open /etc/fstab and comment out (#) the problematic entry:
$ nano /etc/fstab

# 4. Test validity:
$ mount -a

# 5. Reboot back into normal multi-user target:
$ systemctl reboot
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Real World Scenarios](./08-Real-World-Scenarios.md) | [README](./README.md) | [10 - Interview QA](./10-Interview-QA.md) |
