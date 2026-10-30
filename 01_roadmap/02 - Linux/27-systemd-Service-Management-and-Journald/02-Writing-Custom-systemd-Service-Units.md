# 02 — Writing Custom systemd Service Units

Deploying custom microservices (Node.js, Go, Python, Java) into production requires writing robust, self-healing `.service` files.

---

## 1. Unit File Directory Locations

systemd searches for unit files in priority order:
1. `/etc/systemd/system/`: **Administrator-created and custom units** (Highest priority; overrides vendor defaults).
2. `/run/systemd/system/`: Runtime units generated dynamically during boot.
3. `/lib/systemd/system/` or `/usr/lib/systemd/system/`: Distribution package vendor units (Never edit directly; updates will overwrite changes!).

---

## 2. Anatomy of a Production Service Unit

Example: `/etc/systemd/system/api-gateway.service`

```ini
[Unit]
Description=Production API Gateway Microservice
Documentation=https://docs.company.internal/api-gateway
After=network-online.target redis.service
Wants=network-online.target
Requires=redis.service

[Service]
# Execution Type
Type=exec

# Dedicated Non-Root User & Group
User=apiuser
Group=apiuser

# Working Directory & Binary
WorkingDirectory=/opt/api-gateway
ExecStart=/opt/api-gateway/bin/server --config /etc/api-gateway/config.yaml
ExecReload=/bin/kill -HUP $MAINPID

# Environment Variables
Environment=NODE_ENV=production PORT=8080
EnvironmentFile=-/etc/default/api-gateway

# Process Management & Self-Healing
Restart=always
RestartSec=5s
RestartPreventExitStatus=0 2
KillMode=mixed
TimeoutStopSec=30s

# Resource Limits
LimitNOFILE=65536
TasksMax=4096

[Install]
WantedBy=multi-user.target
```

---

## 3. Service `Type=` Explained

| Service Type | How systemd Determines Service Has Started | Use Case |
| :--- | :--- | :--- |
| **`simple`** (Default) | Immediately upon launching the binary (`fork()`) | Standard daemons that do not background themselves |
| **`exec`** | Only after the binary has been successfully loaded into memory | Preferred for modern daemons (detects startup errors immediately) |
| **`forking`** | After the parent process calls `fork()` and exits | Legacy daemons (e.g. Apache, traditional NGINX) |
| **`oneshot`** | After the process runs and terminates cleanly | One-off batch jobs or setup scripts |
| **`notify`** | When the daemon explicitly sends `sd_notify("READY=1")` via Unix socket | Advanced daemons with complex internal initialization |

---

## 4. Managing Unit Lifecycle

```bash
# Reload systemd manager configuration (MANDATORY after creating or editing units!)
sudo systemctl daemon-reload

# Start, stop, restart, and reload
sudo systemctl start api-gateway.service
sudo systemctl stop api-gateway.service
sudo systemctl restart api-gateway.service
sudo systemctl reload api-gateway.service

# Enable at boot and start immediately in one step
sudo systemctl enable --now api-gateway.service

# Check live status, PID, memory, and latest logs
systemctl status api-gateway.service
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - systemd Architecture and PID 1](./01-systemd-Architecture-and-PID-1.md) | [Index](../../../README.md) | [03 - Hardening and Sandboxing Services →](./03-Hardening-and-Sandboxing-Services.md) |
