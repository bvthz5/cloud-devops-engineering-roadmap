# System Signals, Encodings & File Permissions

## 1. POSIX System Signals

System signals are asynchronous notifications sent by the OS kernel or processes to inform a target process of an event.

### Key Linux Signals in DevOps & Containers

| Signal | Number | Description | Container Behavior (`docker stop` vs `kill`) |
|---|---|---|---|
| `SIGHUP` | `1` | Hangup / Reload configuration | Triggers graceful config reload in Nginx/Apache. |
| `SIGINT` | `2` | Terminal Interrupt (Ctrl+C) | Requests process to gracefully interrupt current operation. |
| `SIGQUIT` | `3` | Quit from Keyboard | Requests process to dump core and terminate. |
| `SIGKILL` | `9` | Forceful Kill | **Immediate uncatchable termination by kernel.** Cannot be trapped or ignored! |
| `SIGTERM` | `15` | Termination Request | Default signal sent by `docker stop` / `kubectl delete`. Gives app time to close connections cleanly. |
| `SIGCHLD` | `17` | Child Process Terminated | Sent to parent process when a child exits (handled by PID 1). |

---

## 2. Text Encodings & Line Endings (CRLF vs LF)

Cross-platform development between Windows and Linux frequently leads to script execution failures due to hidden line-ending characters.

### Line Ending Differences
- **Linux / Unix / macOS:** Uses **LF** (`\n`, hex `0x0A`).
- **Windows:** Uses **CRLF** (`\r\n`, hex `0x0D 0x0A`).

### Symptom in Bash Scripts
When a script created on Windows with CRLF is executed on Linux:
```text
/bin/bash^M: bad interpreter: No such file or directory
```

### Resolution Tools
```bash
# Convert Windows CRLF to Linux LF
dos2unix script.sh

# Git config to enforce LF line endings automatically
git config --global core.autocrlf input
```

---

## 3. File Permissions & Octal Representation

Linux file permissions control Read (`r`), Write (`w`), and Execute (`x`) access for User (`u`), Group (`g`), and Others (`o`).

### Octal Value Reference

- `Read (r)` = **4**
- `Write (w)` = **2**
- `Execute (x)` = **1**

### Common Permission Combinations
- `755` (`rwxr-xr-x`): Standard for executable scripts and directories.
- `644` (`rw-r--r--`): Standard for configuration files and source code.
- `600` (`rw-------`): Mandatory for SSH private keys (`id_rsa`).
- `700` (`rwx------`): Restricted home or SSH directory (`~/.ssh`).
