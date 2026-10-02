# 09 — Help Systems, Manual Pages, and CLI Introspection

---

## 1. The Linux Manual (`man` Pages)

The **`man`** command formats and displays the on-line reference manuals installed with operating system packages and software tools.

```bash
man ls
```

### The 8 Standard Manual Sections
Manual pages are organized into 8 numbered sections. When two topics share the same name (e.g., the `passwd` command vs the `/etc/passwd` file), specifying the section number is mandatory:

```text
Section 1: General User Commands (ls, grep, tar, bash)
Section 2: System Calls (fork, read, write, open, clone)
Section 3: C Standard Library Functions (printf, malloc, exit)
Section 4: Special Files & Devices (/dev/null, /dev/tty, /dev/sda)
Section 5: File Formats & Conventions (/etc/passwd, /etc/fstab, crontab)
Section 6: Games & Screensavers
Section 7: Miscellaneous & Protocols (regex, ip, tcp, ascii, capabilities)
Section 8: System Administration Commands (iptables, fdisk, systemctl, mount)
```

```bash
# View manual for the 'passwd' user command (Section 1)
man 1 passwd

# View manual for the '/etc/passwd' configuration file format (Section 5)
man 5 passwd

# View kernel system call manual for 'read' (Section 2)
man 2 read
```

### Searching Manual Pages with `apropos` and `man -k`
When you don't know the exact command name, search manual page descriptions:
```bash
# Search for commands related to disk partitioning
man -k "partition"
# Or:
apropos "partition"
```

---

## 2. GNU `info` System

The GNU Project developed the **`info`** system as a more comprehensive, hyperlinked alternative to standard man pages. It features tree-based navigation with nodes, menus, and cross-references.

```bash
# Open interactive GNU info manual for coreutils
info coreutils
```
- Navigation: `n` (next node), `p` (previous node), `u` (up one level), `q` (quit).

---

## 3. The `--help` Command Flag

Nearly all Linux utilities, cloud CLIs (`aws`, `az`, `gcloud`), and Kubernetes tools (`kubectl`, `helm`) support an internal help flag:

```bash
# Built-in help display
docker container run --help

# Built-in help for shell built-in commands (use 'help', not 'man')
help cd
help export
```

> **Rule of Thumb:**
> - For **shell built-in commands** (`cd`, `pwd`, `read`, `shopt`): Use `help <builtin>`.
> - For **external binaries** (`tar`, `curl`, `find`): Use `man <command>` or `<command> --help`.

---

## 4. Introspection Decision Matrix

| Question | Best Tool | Command Example |
| :--- | :--- | :--- |
| "What kind of command is this? (alias, builtin, file)" | `type` | `type -a kubectl` |
| "Where is the physical binary on disk in $PATH?" | `which` | `which terraform` |
| "Where are the binary, source, and manual files?" | `whereis` | `whereis python3` |
| "How do I run the real binary, ignoring my alias?" | `command` | `command ls` |
| "What command helps me manage network routing?" | `apropos` | `apropos "routing table"` |
| "What are all the CLI flags supported by this tool?" | `--help` | `kubectl apply --help` |
| "How is this configuration file structured?" | `man 5` | `man 5 crontab` |
