# 09 - The /proc Filesystem

All the tools discussed so far (`ps`, `top`, `htop`, `free`) do not magically know what the kernel is doing. They gather their information by reading a special virtual filesystem: **`/proc`**.

Understanding `/proc` allows you to extract deep, raw information about processes without relying on third-party tools.

---

## 👻 What is `/proc`?

It is a pseudo-filesystem. The files inside it do not exist on your hard drive. They are generated on-the-fly by the kernel in RAM when you try to read them. It is a window directly into the kernel's brain.

```bash
ls /proc
```
You will see dozens of numbered directories (`1`, `2`, `1045`, etc.), along with named files like `cpuinfo`, `meminfo`, and `version`.

---

## 📁 Inside a Process Directory

Every running process has a directory in `/proc` matching its PID. If Nginx is running as PID `1045`, you can investigate it by looking inside `/proc/1045`.

```bash
cd /proc/1045
ls
```

### Key Files in `/proc/[PID]/`:

*   **`cmdline`**: The exact command and arguments used to start the process.
    ```bash
    cat cmdline
    # Note: Output is null-separated, so it looks mushed together. Use tr to format:
    cat cmdline | tr '\0' ' '
    ```
*   **`environ`**: The environment variables passed to the process when it started. Extremely useful for debugging applications that can't find their config files or API keys.
    ```bash
    cat environ | tr '\0' '\n'
    ```
*   **`status`**: A highly readable breakdown of the process state, parent PID, user IDs, and memory usage.
    ```bash
    cat status
    ```
*   **`fd/` (Directory)**: Contains symbolic links to all the **F**ile **D**escriptors (open files, sockets, pipes) the process currently holds. This is how `lsof` gets its data.
    ```bash
    ls -l fd/
    ```
*   **`exe`**: A symbolic link pointing to the actual executable binary file on disk.

---

## 💻 System Information in `/proc`

Beyond individual processes, `/proc` exposes global hardware and kernel state.

```bash
# View CPU architecture, cores, and flags
cat /proc/cpuinfo

# Detailed memory usage (this is what `free` parses)
cat /proc/meminfo

# See the exact kernel version running
cat /proc/version

# View all mounted filesystems
cat /proc/mounts
```

---

## 🛠️ DevOps Use Case: Recovering a Deleted File

If someone accidentally deletes a critical file (like a database log), but a process is still actively writing to it, the file isn't truly gone from the disk yet. You can recover it via `/proc`.

1.  Find the PID of the process holding the file open using `lsof | grep deleted`. Let's say PID 2000, File Descriptor 4.
2.  Copy the data out of the `/proc` file descriptor link:
    ```bash
    cp /proc/2000/fd/4 /tmp/recovered_log.txt
    ```
