# 12 - Real-World DevOps Scenarios

Here is how theoretical process management applies to daily operations in a production environment.

---

## 🚨 Scenario 1: The Runaway Process (CPU Spikes)

**The Problem:** Monitoring alerts fire indicating Web Server 01 is at 100% CPU. The application is responding very slowly.

**The Fix:**
1.  **SSH into the server and run `htop` (or `top`).** Sort by CPU (default). You see a process called `image_processor.py` consuming 99% CPU.
2.  **Investigate the process.** Note its PID (e.g., 5542). Run `cat /proc/5542/cmdline | tr '\0' ' '` to see exactly what arguments it was given.
3.  **Assess the impact.** Can this be safely killed? If it's a critical background job, killing it might corrupt data. 
4.  **Throttle it (if critical).** Use `renice` to lower its priority so the web server can breathe.
    ```bash
    sudo renice -n 15 -p 5542
    ```
5.  **Kill it (if stuck/rogue).** If it's stuck in an infinite loop, terminate it cleanly.
    ```bash
    kill 5542
    ```
    *(Wait a few seconds, if still there: `kill -9 5542`)*.

---

## 💥 Scenario 2: Out of Memory (OOM Killer)

**The Problem:** Your Java application suddenly stopped running. You try to restart it, but it immediately dies again.

**The Fix:**
1.  **Check Memory.** Run `free -h`. You see `available` memory is near 0.
2.  **Check System Logs for OOM.** When Linux runs entirely out of RAM and Swap, the kernel invokes the OOM (Out Of Memory) Killer. It hunts down the process using the most RAM and shoots it (SIGKILL) to save the OS.
    ```bash
    dmesg | grep -i oom
    # or
    grep -i "killed process" /var/log/messages
    ```
3.  **Identify the Culprit.** The logs will confirm if your Java app was killed by the OOM Killer. 
4.  **Resolution.** Either the application has a memory leak (requires fixing the code/heap size), or the server genuinely needs more physical RAM provisioned.

---

## 🛑 Scenario 3: The Unkillable Zombie/D-State

**The Problem:** An automated deployment script is hanging. You run `ps` and see a process stuck. You try `kill -9`, but it remains in the process list.

**The Fix:**
1.  **Check the State.** Look at the `STAT` column in `ps aux`.
2.  **If State is 'Z' (Zombie):**
    *   The process is already dead. You cannot kill it.
    *   Find its parent (`ps -ef | grep [PID]`).
    *   Kill the parent process. `systemd` will adopt the zombie and clear it.
3.  **If State is 'D' (Uninterruptible Sleep):**
    *   It is waiting for hardware. `kill -9` will not work.
    *   Run `iostat -x` to check for a failing disk, or check `dmesg` for NFS network timeouts.
    *   If the hardware issue cannot be resolved (e.g., a dead hard drive), the only way to clear a 'D' state process is a server reboot.

---

## 🔁 Scenario 4: The Configuration Reload

**The Problem:** You updated the SSL certificates for Nginx in `/etc/nginx/ssl/`. You need the web server to start using them immediately, but you cannot afford any downtime (restarting the process would drop active user connections).

**The Fix:**
Use the `SIGHUP` signal (Hangup). Most well-written daemons treat `SIGHUP` as a request to re-read their configuration files and gracefully spin up new worker processes without dropping active requests.

```bash
sudo systemctl reload nginx
# Under the hood, systemd essentially runs: kill -HUP $(pidof nginx)
```
