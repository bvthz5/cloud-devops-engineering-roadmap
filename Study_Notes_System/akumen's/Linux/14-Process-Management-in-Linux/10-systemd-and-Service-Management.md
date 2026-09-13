# 10 - systemd and Service Management

Most processes you care about as a DevOps engineer (Nginx, PostgreSQL, Docker) are not started manually in a terminal using `&` or `nohup`. They are started and managed by **systemd**.

---

## ⚙️ What is systemd?

`systemd` is the init system and service manager for almost all modern Linux distributions. 
*   It is **PID 1**.
*   It boots the system, mounts filesystems, manages network connections, and starts all other background services (daemons).
*   If a critical daemon crashes, `systemd` can automatically restart it.

---

## 🛠️ The `systemctl` Command

You interact with `systemd` using the `systemctl` command. 

### Basic Service Control
```bash
# Start a service
sudo systemctl start nginx

# Stop a service
sudo systemctl stop nginx

# Restart a service (stop then start, drops connections)
sudo systemctl restart nginx

# Reload a service (graceful configuration reload, keeps connections alive via SIGHUP)
sudo systemctl reload nginx
```

### Checking Status
This is your primary diagnostic tool when a service fails.
```bash
systemctl status nginx
```
*Look for:*
*   **Active:** active (running) vs failed vs inactive (dead).
*   **Main PID:** The Process ID you would use with `kill` or `strace`.
*   **Recent log lines:** The bottom of the output shows the last few log lines for that service, which often explain *why* it failed.

### Enabling Services on Boot
Just starting a service does not mean it will survive a server reboot. You must explicitly "enable" it to start automatically at boot time.
```bash
sudo systemctl enable nginx
# To disable: sudo systemctl disable nginx
```
*(Pro-tip: You can combine flags: `sudo systemctl enable --now nginx` starts it immediately AND enables it on boot).*

---

## 📝 systemd Unit Files

How does `systemd` know how to start Nginx? It reads a configuration file called a "Unit File".
These are usually located in `/etc/systemd/system/` or `/lib/systemd/system/`.

Example `my-app.service`:
```ini
[Unit]
Description=My Custom Web Application
After=network.target

[Service]
ExecStart=/usr/bin/node /var/www/app/server.js
Restart=always
User=www-data

[Install]
WantedBy=multi-user.target
```

If you modify a unit file, you MUST tell `systemd` to reread the disk before trying to start it:
```bash
sudo systemctl daemon-reload
```

---

## 📖 Viewing Logs with `journalctl`

`systemd` captures standard output (stdout) and standard error (stderr) from all the services it manages and stores them in a centralized binary log called the **journal**.

You view these logs using `journalctl`.

```bash
# View all logs for a specific service (like tail -f)
journalctl -u nginx -f

# View logs since the server last booted
journalctl -b

# View logs for a specific service in the last hour
journalctl -u my-app --since "1 hour ago"
```
