# 11 - Hands-On Practice & Terminal Labs

Follow these step-by-step hands-on terminal exercises to build real muscle memory.

---

## 🧪 Lab 1: Inspecting Inodes, Hard Links, and Symlinks

### Goal:
Understand how directory entries point to inodes and how hard links differ from symbolic links.

```bash
# 1. Create a practice folder and a sample file:
mkdir -p ~/linux_lab && cd ~/linux_lab
echo "DevOps Linux Practice" > original.txt

# 2. Inspect its inode number:
ls -li original.txt
# Output note: The first number is the Inode, link count is 1.

# 3. Create a Hard Link and a Soft (Symbolic) Link:
ln original.txt hardlink.txt
ln -s original.txt symlink.txt

# 4. Compare all three files:
ls -lai original.txt hardlink.txt symlink.txt

# OBSERVE:
# - 'original.txt' and 'hardlink.txt' have the EXACT SAME INODE number!
# - Link count for both increased from 1 to 2.
# - 'symlink.txt' has a completely NEW inode number and its type is 'l'.

# 5. Delete original.txt:
rm original.txt

# 6. Test readability:
cat hardlink.txt   # Still works perfectly! Inode data preserved.
cat symlink.txt    # Broken link error: "No such file or directory"
```

---

## 🧪 Lab 2: Investigating Kernel State Through `/proc`

### Goal:
Directly inspect the Linux kernel and your active shell's runtime internals without installing extra tools.

```bash
# 1. Get your current shell's Process ID ($$):
echo "My Shell PID is: $$"

# 2. Inspect your shell's working directory and executable:
ls -l /proc/$$/cwd
ls -l /proc/$$/exe

# 3. View your process environment variables:
cat /proc/$$/environ | tr '\0' '\n' | head -n 10

# 4. Inspect system hardware statistics:
cat /proc/cpuinfo | grep "model name" | head -n 1
cat /proc/meminfo | grep -E "MemTotal|MemAvailable|SwapTotal"

# 5. Inspect system uptime in days, hours, minutes:
uptime
```

---

## 🧪 Lab 3: Creating & Mounting a Virtual Loopback Disk

### Goal:
Practice partitioning, formatting, and mounting a filesystem safely without needing a spare physical hard drive.

```bash
# 1. Create a 100 Megabyte virtual disk file populated with zeroes:
sudo dd if=/dev/zero of=/tmp/virtual_disk.img bs=1M count=100

# 2. Format the virtual disk image with ext4:
sudo mkfs.ext4 /tmp/virtual_disk.img

# 3. Create a test mount point:
sudo mkdir -p /mnt/looptest

# 4. Mount the virtual disk using loopback:
sudo mount -o loop /tmp/virtual_disk.img /mnt/looptest

# 5. Verify the mount:
df -hT /mnt/looptest

# 6. Create a test file inside the new filesystem:
echo "Hello from mounted filesystem" | sudo tee /mnt/looptest/hello.txt
cat /mnt/looptest/hello.txt

# 7. Unmount and clean up:
sudo umount /mnt/looptest
sudo rm -rf /tmp/virtual_disk.img /mnt/looptest
```

---

## 🧪 Lab 4: Reproducing and Resolving a Deleted Open File

### Goal:
Experience why `rm` on an open log file does not free disk space, and learn how to truncate it live via `/proc`.

```bash
# 1. Create a 200 MB dummy log file:
cd /tmp
dd if=/dev/zero of=/tmp/giant_app.log bs=1M count=200

# 2. Check disk free space before:
df -h /tmp

# 3. Simulate a running process keeping this file open in the background:
tail -f /tmp/giant_app.log > /dev/null &
APP_PID=$!
echo "Process $APP_PID is holding the file open"

# 4. Now delete the file while process is running:
rm -f /tmp/giant_app.log

# 5. Check disk space again:
df -h /tmp
# OBSERVE: The 200 MB is NOT freed!

# 6. Find the open deleted file:
lsof +L1 | grep giant_app.log

# 7. Find the file descriptor under /proc:
ls -l /proc/$APP_PID/fd/
# You will see something like: 3 -> /tmp/giant_app.log (deleted)

# 8. Truncate the file descriptor directly to free the blocks:
truncate -s 0 /proc/$APP_PID/fd/3

# 9. Verify disk space is reclaimed:
df -h /tmp

# 10. Clean up background process:
kill $APP_PID
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Interview QA](./10-Interview-QA.md) | [README](./README.md) | [12 - MCQ](./12-MCQ.md) |
