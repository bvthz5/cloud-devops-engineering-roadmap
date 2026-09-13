# 08 - Troubleshooting & Diagnostic Playbooks

When standard application logs don't reveal the cause of an outage, engineers must drill down through the architectural layers to isolate the issue.

---

## 🧭 Architectural Triage Methodology

Always apply a structured top-down or bottom-up approach across the 5 layers:

```
[Layer 5: App]    ➔ Are application logs reporting exceptions, crashes, or high latency?
[Layer 4: Shell]  ➔ Are systemd service units active? Are processes failing to fork?
[Layer 3: Glibc]  ➔ Are dynamic shared libraries missing? Are syscall wrappers returning errors?
[Layer 2: Kernel] ➔ Is %sy high? Are processes in 'D' state? Did the OOM killer trigger?
[Layer 1: Hardware]➔ Are disks saturated (high %wa)? Is CPU steal time high (%st)? Network drops?
```

---

## 🚨 Issue 1: High System CPU Utilization (`%sy` > 50%)

### Symptom:
`top` or `mpstat` shows the CPU spending the majority of its time in **System Mode (`%sy`)**, while User application time (`%us`) is low.

### Architectural Cause:
The system is bottlenecked by the kernel executing an excessive frequency of **System Calls** or encountering severe lock contention (such as `futex` lock contention in multi-threaded runtimes).

### Diagnostic Procedure:
```bash
# 1. Profile system calls across the entire system for 10 seconds:
$ sudo perf top

# 2. Record call graph for 5 seconds to generate flame graphs:
$ sudo perf record -F 99 -a -g -- sleep 5
$ sudo perf report

# 3. If a specific PID is suspected, count its syscall distribution:
$ sudo strace -c -p <PID>
```
*Look for:* Thousands of `futex()`, `epoll_wait()`, or `mprotect()` calls per second indicating memory thrashing or thread synchronization deadlocks.

---

## 🚨 Issue 2: Process Stuck in State `D` (Uninterruptible Sleep)

### Symptom:
A process is unresponsive. You execute `sudo kill -9 <PID>`, but the process stubbornly refuses to die and remains listed in `ps` output with status **`D`**:
```
USER   PID  %CPU %MEM   VSZ   RSS TTY   STAT START   TIME COMMAND
dbuser 8912  0.0  1.2 51200 24000 ?     D    10:00   0:00 /usr/bin/postgres
```

### Architectural Cause:
In Linux, **State `D`** means the process is blocked waiting for a **hardware I/O operation** (usually reading from a stalled disk, a frozen NFS mount, or an unresponsive SAN).
Because the process is sleeping inside a kernel device driver routine, **the kernel disables signal delivery** to prevent filesystem corruption. Even `SIGKILL` (signal 9) will not take effect until the hardware I/O request finishes or times out!

### Diagnostic Procedure:
```bash
# 1. Find the exact kernel function where the process is blocked:
$ cat /proc/8912/wchan
rpc_wait_bit_killable  # (Indicates it is stuck waiting on an NFS network RPC call!)

# 2. View the full kernel execution stack trace:
$ sudo cat /proc/8912/stack
[<0>] rpc_wait_bit_killable+0x24/0xa0 [sunrpc]
[<0>] nfs4_proc_lookup_common+0x180/0x240 [nfsv4]
[<0>] vfs_open+0x31/0x90
[<0>] do_sys_openat2+0x9b/0x150
[<0>] __x64_sys_openat+0x54/0x90
[<0>] do_syscall_64+0x5c/0x90

# 3. Actionable Fix:
# Do not reboot yet! Check the network connectivity or unmount the hung NFS share with force:
$ sudo umount -f -l /mnt/nfs_share
```

---

## 🚨 Issue 3: Missing Shared Libraries (`error while loading shared libraries`)

### Symptom:
A developer transfers a compiled binary to a production server, runs it, and receives:
```
./my_microservice: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory
```

### Architectural Cause:
The binary was compiled with dynamic linking (Layer 3). When executed, the dynamic linker (`ld-linux.so`) searched `/lib` and `/usr/lib` but could not find the specified dynamic shared object (`.so`).

### Diagnostic & Fix:
```bash
# 1. Inspect all missing dependencies using 'ldd':
$ ldd ./my_microservice
	linux-vdso.so.1 (0x00007ffca7bdf000)
	libssl.so.1.1 => not found              <-- Missing!
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fca...)

# 2. Check the system's dynamic linker search cache:
$ ldconfig -p | grep libssl

# 3. Temporary Fix (Specify library path in user space):
$ export LD_LIBRARY_PATH=/opt/custom_libs/lib:$LD_LIBRARY_PATH

# 4. Permanent Fix:
# Add custom library path to /etc/ld.so.conf.d/ and refresh linker cache:
$ echo "/opt/custom_libs/lib" | sudo tee /etc/ld.so.conf.d/custom.conf
$ sudo ldconfig
```
