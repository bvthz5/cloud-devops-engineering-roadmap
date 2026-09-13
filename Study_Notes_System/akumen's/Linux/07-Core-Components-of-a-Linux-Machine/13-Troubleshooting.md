# 13 - Troubleshooting Methodology for Linux Component Failures

A structured layer-by-layer diagnostic methodology for isolating component failures on a Linux machine.

---

## 🔍 Systematic Layer-by-Layer Diagnostic Matrix

```text
[ Layer 6: User App ]    ---> Is process running? (`ps aux | grep app`, `systemctl status`)
[ Layer 5: Shell ]       ---> Is syntax or PATH correct? (`echo $PATH`, `type command`)
[ Layer 4: Utility ]     ---> Are permissions & dependencies met? (`ls -l`, `ldd /path/to/bin`)
[ Layer 3: System Libs ] ---> Is glibc or shared object missing? (`ldd`, `strace -e openat`)
[ Layer 2: Kernel ]      ---> Are kernel logs reporting panics/OOM? (`dmesg -T`, `/var/log/syslog`)
[ Layer 1: Hardware ]    ---> Are CPU, RAM, or disks failing? (`lscpu`, `smartctl`, `free -h`)
```

---

## 🛠️ Step-by-Step Diagnostic Workflow

### Step 1: Check Process State & Logs
```bash
# Verify process status
systemctl status nginx

# View process error logs via journalctl
journalctl -u nginx -n 50 --no-pager
```

### Step 2: Trace System Calls (`strace`)
If an executable binary fails silently without error logs:
```bash
# Trace system call execution of target command
strace -f -o /tmp/trace.log /usr/bin/custom_app

# Grep trace log for failed system calls
grep -i "ENOENT\|EACCES\|EPERM" /tmp/trace.log | head -n 20
```

### Step 3: Inspect Shared Library Dependencies (`ldd`)
```bash
# Check if any dynamically linked shared object (.so) is missing
ldd /usr/bin/custom_app | grep "not found"
```

---

## ⬅️ Navigation
- Previous: [12 - Real-World Production Scenarios](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/12-Real-World-Scenarios.md)
- Next: [14 - Interview Q&A](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/14-Interview-QA.md)
