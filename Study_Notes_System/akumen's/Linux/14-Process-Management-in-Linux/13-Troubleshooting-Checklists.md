# 13 - Troubleshooting Checklists

When responding to an incident, follow these checklists to systematically isolate the problem rather than guessing randomly.

---

## 🟥 Checklist: High System Load / Slow Response

1.  **Check the Load Average:**
    *   Command: `uptime` or `top`
    *   *Action:* Is the 1-minute load average significantly higher than your CPU core count (`nproc`)? If yes, the system is overloaded.
2.  **Is it a CPU bottleneck?**
    *   Command: `htop` (Sort by `CPU%`)
    *   *Action:* If a process is consuming ~100% CPU, note its PID.
    *   *Resolution:* Attempt to restart the service (`systemctl restart`), throttle it (`renice`), or kill it (`kill -15`, then `kill -9`).
3.  **Is it a Memory bottleneck?**
    *   Command: `free -h`
    *   *Action:* Is `available` memory near zero? Is the system heavily using `Swap`?
    *   *Resolution:* Use `htop` (Sort by `MEM%`) to find the memory hog. If it's a memory leak, restart it. Check `dmesg` for OOM Killer activity.
4.  **Is it a Disk I/O bottleneck?**
    *   Command: `iostat -xz 1` (Look at `%util`) or `iotop`
    *   *Action:* Is the disk utilization near 100% despite low CPU usage? Are many processes in the `D` (Uninterruptible Sleep) state in `ps aux`?
    *   *Resolution:* Find out what is writing/reading so heavily. It could be a runaway logging process or a failing disk.

---

## 🟧 Checklist: A Service Will Not Start

1.  **Check the Status:**
    *   Command: `systemctl status <service_name>`
    *   *Action:* Look at the bottom few lines. What does the error message say? Is it a syntax error in a config file? A port binding error?
2.  **Check the Full Logs:**
    *   Command: `journalctl -u <service_name> -e` (Jumps to the end of the logs).
    *   *Action:* Look for the exact reason it crashed during startup.
3.  **Check for Port Conflicts:**
    *   Command: `sudo lsof -i :<expected_port>` or `netstat -tulpn | grep <port>`
    *   *Action:* If starting a web server on port 80 fails, see if another process (like Apache or a rogue Docker container) is already bound to port 80.
4.  **Check Permissions:**
    *   *Action:* Does the user defined in the `systemd` service file have read access to the config files and write access to the log directories? (See Permissions Module).
5.  **Test Configuration Syntax manually:**
    *   Most daemons have a "test config" flag.
    *   *Examples:* `nginx -t`, `apache2ctl configtest`, `sshd -t`.

---

## 🟨 Checklist: Process Exits Immediately When User Disconnects

1.  **How was it started?**
    *   If you started it with `./script.sh &` and then closed the terminal, it was killed by `SIGHUP`.
2.  **The Fix:**
    *   Option A (Quick): Start it with `nohup ./script.sh &`.
    *   Option B (Interactive): Run it inside a `tmux` session, then detach.
    *   Option C (Permanent): If this script should always be running in the background across reboots, write a `systemd` unit file for it.

---

## 🟩 Checklist: Out of Disk Space (But Files Were Deleted)

1.  **The Symptom:**
    *   `df -h` says the disk is 100% full.
    *   You delete a massive 50GB log file: `rm /var/log/app.log`.
    *   `df -h` STILL says the disk is 100% full.
2.  **The Cause:**
    *   In Linux, a file is not actually removed from the disk until two conditions are met:
        1. The link is removed from the directory (which `rm` does).
        2. **All processes holding a file descriptor to that file close it.**
    *   If the application is still running, it holds the file open, and the disk space is not freed.
3.  **The Fix:**
    *   Command: `sudo lsof | grep deleted`
    *   *Action:* Find the process (PID) holding the deleted file open.
    *   *Resolution:* Restart that specific service (`systemctl restart <service>`) or kill the PID. The kernel will instantly reclaim the 50GB.
    *   *Future Prevention:* Don't `rm` active logs. Truncate them instead: `> /var/log/app.log`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Real World DevOps Scenarios](./12-Real-World-DevOps-Scenarios.md) | [README](./README.md) | [14 - Interview QA](./14-Interview-QA.md) |
