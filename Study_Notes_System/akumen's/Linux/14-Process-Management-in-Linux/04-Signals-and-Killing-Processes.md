# 04 - Signals and Killing Processes

In Linux, you do not directly force a process to stop. Instead, the kernel provides an Inter-Process Communication (IPC) mechanism called **Signals**. You send a signal to a process, and the process responds accordingly.

The `kill` command is poorly named; a more accurate name would be `send_signal`. While its primary use is terminating processes, it is used for much more.

---

## 📡 The Most Important Signals

There are 64 standard signals, but you only need to memorize these five:

| Signal Name | Number | Description | Can the process block/ignore it? |
| :--- | :---: | :--- | :---: |
| **SIGTERM** | **15** | **Terminate.** The default signal sent by `kill`. It politely asks the process to shut down. | **Yes.** The process can catch it, clean up temporary files, save state, and exit gracefully. |
| **SIGKILL** | **9** | **Kill.** The absolute force-quit. The kernel immediately destroys the process. | **No.** The process cannot catch, block, or ignore it. (Except 'D' state processes). |
| **SIGHUP** | **1** | **Hangup.** Originally meant the terminal connection dropped. Now commonly used to tell background daemons (like Nginx) to reload their configuration files without restarting. | **Yes.** |
| **SIGINT** | **2** | **Interrupt.** Sent when you press `Ctrl+C` in the terminal. Asks the foreground job to stop. | **Yes.** |
| **SIGSTOP** | **19** | **Stop / Pause.** Sent when you press `Ctrl+Z`. Pauses the process, putting it in the 'T' state. | **No.** |
| **SIGCONT** | **18** | **Continue.** Wakes up a stopped ('T' state) process. | **No.** |

---

## 🔫 Sending Signals with `kill`

The `kill` command requires the **PID** of the target process.

### 1. The Polite Shutdown (SIGTERM / 15)

If you don't specify a signal, `kill` sends `SIGTERM` (15) by default.
```bash
kill 1045
# Equivalent to: kill -15 1045
# Equivalent to: kill -TERM 1045
```
**Best Practice:** Always try this first. Give the application a chance to save its data and close database connections cleanly.

### 2. The Force Quit (SIGKILL / 9)

If a process is frozen, deadlocked, or ignoring `SIGTERM`, escalate to `SIGKILL`.
```bash
kill -9 1045
# Equivalent to: kill -KILL 1045
```
**Warning:** This rips the process out of memory instantly. Temporary files are left behind, and data might be corrupted. Use as a last resort.

---

## 🎯 Targeting by Name: `pkill` and `killall`

Finding the PID with `ps` and then typing it into `kill` is tedious. 

### `pkill` (Pattern Kill)
Sends a signal to processes based on a regex pattern or name. It is the exact equivalent of running `pgrep` and piping the output to `kill`.

```bash
# Politely terminate all processes containing "nginx"
pkill nginx

# Force kill all processes owned by the user 'alice'
pkill -9 -u alice
```

### `killall`
Similar to `pkill`, but usually requires an **exact** match of the process name (depending on the OS implementation).

```bash
# Tell all nginx worker processes to reload their config (SIGHUP)
killall -HUP nginx
```

---

## ⚖️ The "Kill -9" Golden Rule

A common mistake among junior administrators is instantly reaching for `kill -9` whenever a process misbehaves. 

**The Rule of Escalation:**
1. Send `SIGTERM` (the default `kill PID`).
2. Wait 3 to 5 seconds. Check if the process exited.
3. If it hasn't exited, *then* send `SIGKILL` (`kill -9 PID`).

If you use `kill -9` on a database like PostgreSQL or MySQL, you risk severe database corruption because the engine cannot flush its writes to disk.
