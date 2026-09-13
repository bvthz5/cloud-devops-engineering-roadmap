# 06 - Detaching Processes: nohup and tmux

As established in the Job Control chapter, if you close your terminal or your SSH connection drops, the kernel sends a `SIGHUP` (Hangup) signal to your shell. The shell then passes this `SIGHUP` to all its child processes (including background jobs), killing them.

DevOps engineers frequently run long tasks (database imports, large file transfers, script executions). You must protect these processes from terminal disconnects.

---

## 🛡️ Method 1: The Quick Fix (`nohup`)

`nohup` stands for "No Hangup". It intercepts the `SIGHUP` signal and ignores it, ensuring the command continues running even if the terminal dies.

### Syntax
```bash
nohup command &
```
*(You almost always combine `nohup` with `&` so it runs in the background).*

### Example
```bash
nohup ./long_database_import.sh &
```

### Where does the output go?
Because you might close the terminal, `nohup` automatically redirects standard output (stdout) and standard error (stderr) to a file named `nohup.out` in the current directory.

You can redirect it manually for better organization:
```bash
nohup ./long_database_import.sh > import.log 2>&1 &
```

### The Limitation of nohup
Once you start a process with `nohup` and close the terminal, you **cannot reattach** to it later to interact with it. You can only view its progress by tailing the log file (`tail -f import.log`), and you can only stop it by finding its PID and using `kill`.

---

## ✂️ Method 1.5: The Post-Hoc Fix (`disown`)

What if you started a command normally, realized it's going to take hours, put it in the background with `Ctrl+Z` and `bg`, but now you need to log out?

You didn't use `nohup`, so if you log out, it will die. 

Use `disown` to detach a running job from the shell's job list.

```bash
# 1. Start the job
./long_script.sh

# 2. Realize it takes too long. Pause it.
Ctrl+Z

# 3. Send it to the background.
bg

# 4. Remove it from the shell's control.
disown %1
```
Now you can safely exit the SSH session, and the script will continue running.

---

## 🪟 Method 2: The Professional Standard (`tmux`)

`tmux` (Terminal Multiplexer) is the modern standard for managing persistent terminal sessions. (An older alternative is `screen`).

Instead of protecting a single command from `SIGHUP`, `tmux` creates a virtual terminal session on the server. You attach to this virtual session, run your commands normally, and then **detach**. The virtual session keeps running on the server. Later, you can SSH back in and **attach** to exactly where you left off.

### Basic Workflow

**1. Start a new tmux session:**
```bash
tmux new -s db_import
```
*(Your screen will clear, and a green status bar will appear at the bottom. You are now inside tmux).*

**2. Run your long command:**
```bash
./long_database_import.sh
```

**3. Detach from the session:**
Press `Ctrl+b`, release both keys, then press `d`.
*(You are dropped back to your normal SSH shell. The script is still running inside tmux).*

**4. Log out, go get coffee, log back in via SSH.**

**5. List running tmux sessions:**
```bash
tmux ls
# Output: db_import: 1 windows (created Mon Sep 14 10:00:00 2026)
```

**6. Re-attach to your session:**
```bash
tmux attach -t db_import
```
*(You are back looking at your script exactly where you left it).*

### Why use tmux?
For any task that takes longer than 5 minutes, or for any task where a dropped VPN connection would cause a disaster, `tmux` is mandatory.
