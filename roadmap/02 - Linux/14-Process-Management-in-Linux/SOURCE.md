# Process Management in Linux

## Purpose
Master the lifecycle, monitoring, and control of Linux processes to maintain system stability and performance.

```text
The Process Core
PID | State | Priority
top/ps | kill | nice/renice
```

## Source Foundation

The supplied material covers the fundamentals of process architecture (PID, PPID, UID), process states (Running, Sleeping, Stopped, Zombie), viewing tools (`ps`, `top`, `htop`, `pgrep`), process communication via signals (`kill`, `SIGTERM`, `SIGKILL`), shell job control, detachment mechanisms (`nohup`, `tmux`), priority scheduling (`nice`), resource monitoring, the `/proc` filesystem, `systemd` service management, and cron scheduling.

The original source concepts have been expanded and organized into this structured study module with clearly identified learning expansions, real-world troubleshooting scenarios, and practice labs.

## Rule of thumb
*   Need to see what's eating CPU? → `htop`
*   Need to politely stop a process? → `kill -15 PID`
*   Need to forcefully kill a frozen process? → `kill -9 PID`
*   Need a long script to survive disconnect? → `tmux`

## Learning path
*   Process fundamentals — PID, PPID, UID, parent/child
*   Process states — `R`, `S`, `D`, `T`, `Z`
*   `ps`, `top`, `htop`, `pgrep`, `pidof`, `pstree`
*   Signals — `SIGTERM`, `SIGKILL`, `SIGHUP`, `SIGSTOP`, `SIGCONT`
*   `kill`, `pkill`, `killall`
*   Foreground/background jobs
*   `jobs`, `bg`, `fg`
*   `nohup`, `disown`, `tmux`
*   `nice` and `renice`
*   CPU, memory, disk I/O and load monitoring
*   `free`, `vmstat`, `iostat`, `uptime`, `lsof`
*   `/proc` filesystem and process investigation
*   systemd, `systemctl`, `journalctl`
*   `at` and cron scheduling
*   Real-world DevOps scenarios
*   Troubleshooting checklists
*   Interview questions & answers
*   Hands-on terminal exercises
*   MCQs
*   Quick revision
*   Related advanced topics
