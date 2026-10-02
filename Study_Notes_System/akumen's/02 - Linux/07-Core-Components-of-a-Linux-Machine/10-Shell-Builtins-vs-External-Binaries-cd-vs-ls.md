# 10 - Shell Built-ins vs. External Binaries (`cd` vs `ls`)

A foundational distinction in Linux execution mechanics is understanding why certain commands are **Shell Built-ins** while others are **External Executable Binaries**.

---

## 🆚 Built-in vs. External Binary Comparison

| Feature | Shell Built-in Command (e.g., `cd`, `echo`, `export`) | External Binary Utility (e.g., `ls`, `grep`, `cat`) |
| :--- | :--- | :--- |
| **Execution Location** | Executed directly inside the running Shell process memory space. | Executed in a separate child process after `fork()` and `execve()`. |
| **Binary File on Disk?** | No standalone file exists on disk. | Yes. Located at `/usr/bin/ls`, `/usr/bin/grep`, etc. |
| **Performance Overhead** | Ultra-fast. No process creation (`fork`) overhead. | Slightly higher overhead due to process creation and dynamic library loading. |
| **Modifies Shell Environment?** | Can directly alter working directory (`cd`), environment variables (`export`), or limits (`ulimit`). | Cannot alter parent shell environment (process child memory isolation). |

---

## 🔍 The Practical Distinction: Why `cd` MUST be a Shell Built-in

Suppose `cd` were written as an external binary executable file located at `/usr/bin/cd`:

1. When a user types `cd /var/log`, the shell would call `fork()` to create a child process.
2. The child process would execute `/usr/bin/cd /var/log`, calling the kernel system call `chdir("/var/log")`.
3. The kernel would successfully change the working directory of the **child process**.
4. The child process would exit.
5. The parent shell process would remain in its original directory!

Because a child process in Linux **cannot modify the current working directory of its parent process**, `cd` **must** be executed directly inside the parent shell process as a shell built-in function.

Contrast this with `ls`: `ls` simply reads directory entries and prints text to stdout. It does not need to alter shell environment state, making it ideal as an external utility (`/usr/bin/ls`).

---

## 🛠️ Identifying Command Types in the Terminal

```bash
# Check type of 'cd'
type cd
# Output: cd is a shell builtin

# Check type of 'ls'
type ls
# Output: ls is aliased to `ls --color=auto`

# Check unaliased type of 'ls'
type -t ls
# Output: file (meaning an external executable file on disk)
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Command Flow ls cat networking](./09-Command-Flow-ls-cat-networking.md) | [README](./README.md) | [11 - Practical Commands](./11-Practical-Commands.md) |
