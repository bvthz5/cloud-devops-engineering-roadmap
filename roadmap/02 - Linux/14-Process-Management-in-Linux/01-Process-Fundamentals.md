# 01 - Process Fundamentals

At its core, a Linux system is simply a collection of executing programs managed by the kernel. Understanding how the kernel tracks and organizes these programs is the first step to mastering process management.

---

## 🖥️ What is a Process?

A **program** is an executable file resting on the disk (e.g., `/usr/bin/ls`). 
A **process** is the executing instance of that program in memory. 

When you run a command, the Linux kernel loads the program's code into memory, allocates resources (CPU time, RAM, file descriptors), and creates a process.

---

## 🆔 Process Identifiers (PIDs)

The kernel needs a way to track every running process. It does this by assigning a unique, non-negative integer called a **Process ID (PID)**.

*   **PID 1:** The very first process started by the kernel during boot. Historically this was `init`, but in modern systems, it is almost always `systemd`. PID 1 is the ancestor of all other processes.
*   **PID Assignment:** PIDs are assigned sequentially. When the maximum PID is reached (historically 32,768, but often much higher on modern systems), the kernel wraps around and reuses available low numbers.

---

## 👨‍👦 Parent and Child Processes

Linux processes are organized in a strict hierarchical tree structure.

When a process (the **parent**) starts another process (the **child**), it does so using a system call called `fork()`. 
*   The `fork()` creates an exact duplicate of the parent process.
*   The child process then usually runs `exec()`, which replaces its memory space with a new program.

### PPID (Parent Process ID)
Every process (except PID 1) has a parent. The kernel tracks this using the **PPID**.
If you open a bash terminal (e.g., PID 1000) and run the `ls` command (e.g., PID 1050), then:
*   `ls` has a PID of 1050 and a PPID of 1000.
*   `bash` is the parent of `ls`.

### Orphans and the `init`/`systemd` Adoption
If a parent process dies before its child, the child becomes an **orphan**. The Linux kernel does not allow orphans to exist without a parent. It immediately reassigns the orphan's PPID to PID 1 (`systemd` or `init`). PID 1 "adopts" the orphan.

---

## 👤 User and Group IDs (UID/GID)

Processes don't exist in a vacuum; they execute on behalf of a user.

*   **UID (User ID):** The user who started the process. The process runs with the permissions of this user (unless special permissions like SUID are set, see permissions module).
*   **GID (Group ID):** The primary group of the user who started the process.

If a process running as `alice` (UID 1001) tries to read a file owned by `root` with `600` permissions, the kernel denies access based on the process's UID.

---

## 🌳 Viewing the Process Tree

To visualize the parent-child relationship, you can use the `pstree` command.

```bash
pstree
```
*Output snippet:*
```text
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─chronyd
        ├─sshd───sshd───bash───pstree
        └─systemd-journal
```
In this example, `systemd` spawned `sshd` (the SSH server). When you connected, that `sshd` spawned a child `sshd` session, which spawned your `bash` shell, which you just used to run `pstree`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Process States](./02-Process-States.md) |
