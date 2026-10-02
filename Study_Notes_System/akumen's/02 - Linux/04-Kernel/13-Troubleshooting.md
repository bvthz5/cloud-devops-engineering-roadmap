# 13 - Kernel Troubleshooting & Diagnostic Playbooks

When a software bug reaches the kernel layer, standard debugging utilities cease to function. Engineers must rely on kernel ring buffers, crash dumps, and sysctl watchdogs.

---

## 🚨 Incident 1: Diagnosing a Kernel Panic

### The Symptom:
A production server suddenly freezes, drops all SSH sessions, and prints a crash dump to the cloud serial console ending with:
```
[ 142.102391] Kernel panic - not syncing: Fatal exception
[ 142.103011] CPU: 2 PID: 4812 Comm: custom_driver Tainted: G           OE     6.5.0 #1
[ 142.104102] Call Trace:
[ 142.104500]  <TASK>
[ 142.104800]  custom_driver_write+0x42/0x90 [custom_driver]
[ 142.105200]  vfs_write+0x9b/0x240
[ 142.105600]  ksys_write+0x54/0xd0
```

### The Architectural Cause:
A **Kernel Panic** is the Linux kernel's fail-safe mechanism when an unrecoverable fatal error occurs while executing inside **Ring 0**. The kernel immediately halts CPU processing to prevent silent data destruction across physical disks.
- Common causes: Null-pointer dereference inside a device driver, hardware memory bit-flips, or failure to mount the root filesystem at boot.

### Production Recovery & Prevention:
1. **Enable Automatic Reboot on Panic:** By default, Linux halts forever on a panic. In cloud and automated clusters, configure the kernel to automatically reboot after 10 seconds:
   ```bash
   $ sudo sysctl -w kernel.panic=10
   # Persist in /etc/sysctl.conf:
   kernel.panic = 10
   ```
2. **Inspect Kernel Crash Dumps (`kdump`):**
   `kdump` uses a tiny secondary recovery kernel to capture the complete memory state (`vmcore`) of the crashed kernel and write it to `/var/crash/` for post-mortem analysis with the `crash` tool.

---

## 🚨 Incident 2: "Blocked for more than 120 seconds" (Hung Task)

### The Symptom:
`dmesg` outputs repeated warnings:
```
echo 0 > /proc/sys/kernel/hung_task_timeout_secs
[Sun Sep 13 14:30:00 2026] INFO: task java:18402 blocked for more than 120 seconds.
[Sun Sep 13 14:30:00 2026]       Not tainted 6.5.0-35-generic #35
[Sun Sep 13 14:30:00 2026] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
```

### The Cause:
The process has remained stuck in **State `D` (Uninterruptible Sleep)** for over 120 seconds without completing its I/O request. The kernel's `hung_task` watchdog timer detected the deadlock.

### Diagnostic & Triage:
```bash
# 1. View the exact kernel call trace of the stuck task:
$ sudo cat /proc/18402/stack

# 2. Check the wait channel:
$ cat /proc/18402/wchan
io_schedule  # (Stuck waiting for storage disk hardware blocks)

# 3. Check disk queue latency:
$ iostat -xz 1 3
```

---

## 🚨 Incident 3: Blacklisting Faulty or Conflicting Kernel Modules

If a faulty driver or third-party module causes crashes, prevent the kernel from loading it during boot:

```bash
# Create a blacklist configuration file:
$ echo "blacklist nouveau" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
$ echo "options nouveau modeset=0" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf

# Rebuild the initial RAM disk (initramfs) so the blacklist applies at early boot:
$ sudo update-initramfs -u
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Real World Scenarios](./12-Real-World-Scenarios.md) | [README](./README.md) | [14 - Interview Q&A](./14-Interview-Q&A.md) |
