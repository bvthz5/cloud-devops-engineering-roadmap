# 05 - Job Control: fg, bg, and jobs

A single Linux terminal session (shell) can manage multiple processes simultaneously through a feature called **Job Control**. 

A "job" is simply a process (or pipeline of processes) initiated by your shell. 

---

## 🎭 Foreground vs. Background

*   **Foreground Job:** The command takes over your terminal. You cannot type new commands until the job finishes or is interrupted. (e.g., running `top` or a long `tar` backup).
*   **Background Job:** The command runs behind the scenes. The shell immediately gives you back your prompt so you can continue working.

---

## 🚀 Starting a Job in the Background (`&`)

To start a command in the background, append an ampersand (`&`) to the end of the line.

```bash
sleep 300 &
# Output: [1] 23456
```
The shell responds with `[1]` (the Job ID) and `23456` (the Process ID). You immediately get your prompt back.

---

## ⏸️ Suspending a Foreground Job (`Ctrl+Z`)

Imagine you started a long-running backup, but forgot to put it in the background:
```bash
tar -czvf backup.tar.gz /var/log/
```
Your terminal is blocked. You don't want to cancel it (with `Ctrl+C`). Instead, you can **suspend** it.

Press `Ctrl+Z`.

The shell sends a `SIGSTOP` signal to the process, putting it in the 'T' (Stopped) state, and returns your prompt.
```text
[1]+  Stopped                 tar -czvf backup.tar.gz /var/log/
```

---

## 📋 Viewing Jobs (`jobs`)

To see all jobs currently managed by your shell session:
```bash
jobs
```
*Output:*
```text
[1]-  Running                 sleep 300 &
[2]+  Stopped                 tar -czvf backup.tar.gz /var/log/
```
*   `[1]` and `[2]` are the **Job IDs** (not PIDs).
*   The `+` indicates the default job (the most recently accessed one).
*   The `-` indicates the next-in-line job.

---

## 🔄 Resuming Jobs (`bg` and `fg`)

### Sending to the Background (`bg`)
You suspended your `tar` backup with `Ctrl+Z`. It is currently doing nothing (Stopped). To tell it to resume execution, but run in the background, use `bg` followed by the Job ID (prepended with `%`).

```bash
bg %2
```
The process state changes from 'T' (Stopped) to 'S/R' (Sleeping/Running), and it continues backing up files while you use the terminal.

### Bringing to the Foreground (`fg`)
If you want to bring a background job back to the foreground so you can interact with it (or just watch it finish), use `fg`.

```bash
fg %1
```
The `sleep 300` command now takes over your terminal again.

---

## ⚠️ The Limitation of Job Control

Job Control is bound to your specific SSH/Terminal session. 

If you have jobs running in the background and you close your terminal (or your SSH connection drops), the shell sends a `SIGHUP` (Hangup) signal to all its child jobs, terminating them instantly.

To run processes that survive a disconnect, you need tools like `nohup` or `tmux` (covered in Chapter 6).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Signals and Killing Processes](./04-Signals-and-Killing-Processes.md) | [README](./README.md) | [06 - Detaching Processes nohup tmux](./06-Detaching-Processes-nohup-tmux.md) |
