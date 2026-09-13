# 10 - Hands-On Practice & Architecture Labs

Execute these step-by-step terminal experiments to observe system architecture, kernel modules, and system calls live.

---

## 🧪 Lab 1: Tracing the `cp` Command System Calls with `strace`

### Objective:
Observe the exact file descriptors and syscall sequence during file copy.

```bash
# 1. Create a directory and test file:
mkdir -p ~/arch_lab && cd ~/arch_lab
echo "Hello Linux Architecture" > sample.txt

# 2. Run strace filtering specifically for file manipulation syscalls:
strace -e trace=openat,read,write,close,copy_file_range cp sample.txt copy.txt

# 3. Analyze the terminal output:
# - Look for: openat(..., "sample.txt", O_RDONLY) = 3
# - Look for: openat(..., "copy.txt", O_WRONLY|O_CREAT...) = 4
# - Look for: copy_file_range(3, ..., 4, ...) OR read(3, ...)/write(4, ...)
# - Look for: close(4) and close(3)

# 4. Count total syscalls invoked by cp:
strace -c cp sample.txt copy2.txt
```

---

## 🧪 Lab 2: Managing Loadable Kernel Modules (LKMs)

### Objective:
Dynamically load, inspect, and remove a kernel module without rebooting.

```bash
# 1. Check if the 'dummy' network module is loaded:
lsmod | grep dummy

# 2. Inspect the metadata of the dummy kernel module:
modinfo dummy

# 3. Insert the module into the running kernel:
sudo modprobe dummy

# 4. Verify the module is now loaded:
lsmod | grep dummy

# 5. Notice the kernel created a new virtual network interface!
ip link show dummy0

# 6. Unload the module from the kernel:
sudo modprobe -r dummy

# 7. Verify the interface was automatically destroyed:
ip link show dummy0
# (Should return: Device "dummy0" does not exist)
```

---

## 🧪 Lab 3: Inspecting Process Virtual Memory Mappings

### Objective:
Inspect how system libraries (glibc) and memory segments are mapped into a running process address space.

```bash
# 1. Inspect your current shell's process ID:
MY_PID=$$
echo "Inspecting PID: $MY_PID"

# 2. View the virtual memory address segments:
cat /proc/$MY_PID/maps | head -n 20

# OBSERVE:
# - The executable binary segment (r-xp)
# - The heap segment ([heap])
# - The loaded C standard library (libc.so.6)
# - The thread stack segment ([stack])
# - The kernel virtual dynamic shared object ([vdso])
```

---

## 🧪 Lab 4: Observing Page Cache and Dirty Pages

### Objective:
See how the Linux kernel uses free RAM to cache disk I/O and how dirty pages behave.

```bash
# 1. Check initial memory and page cache state:
free -h
cat /proc/meminfo | grep -E "Cached|Dirty"

# 2. Write a 200 MB file directly to disk:
dd if=/dev/zero of=testfile.img bs=1M count=200

# 3. Check memory again:
cat /proc/meminfo | grep -E "Cached|Dirty"
# (Notice that 'Cached' increased by ~200 MB!)

# 4. Flush dirty pages from RAM to physical disk:
sync

# 5. Tell the kernel to drop clean caches:
sudo bash -c 'echo 3 > /proc/sys/vm/drop_caches'

# 6. Check memory again:
cat /proc/meminfo | grep -E "Cached|Dirty"
# (Notice 'Cached' dropped significantly!)

# 7. Clean up:
rm -f testfile.img
```
