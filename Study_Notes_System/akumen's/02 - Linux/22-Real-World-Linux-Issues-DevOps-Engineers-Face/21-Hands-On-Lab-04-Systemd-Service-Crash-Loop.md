# 21 — Hands-On Lab 04: Systemd Service Crash Loop

## Lab Objective
Create a broken systemd service with an invalid `ExecStart` path, observe its crash status via `systemctl`, diagnose using `journalctl`, fix the service configuration, and restore full operational state.

## Step-by-Step Instructions

### Step 1: Create Broken Systemd Service Unit
```bash
sudo cat <<'EOF' > /etc/systemd/system/broken_test.service
[Unit]
Description=Broken Test Daemon
After=network.target

[Service]
Type=simple
ExecStart=/nonexistent/path/binary
Restart=on-failure
RestartSec=1s

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl start broken_test.service
```

### Step 2: Diagnose Failure
```bash
# Check service status
sudo systemctl status broken_test.service

# Check journal log for error exit code (status=203/EXEC)
sudo journalctl -u broken_test.service -n 20 --no-pager
```

### Step 3: Fix Service Configuration
```bash
# Update ExecStart to valid binary (/usr/bin/sleep 30)
sudo sed -i 's|/nonexistent/path/binary|/usr/bin/sleep 30|' /etc/systemd/system/broken_test.service

# Reload systemd manager & restart
sudo systemctl daemon-reload
sudo systemctl restart broken_test.service
sudo systemctl status broken_test.service
```

### Step 4: Cleanup
```bash
sudo systemctl stop broken_test.service
sudo rm -f /etc/systemd/system/broken_test.service
sudo systemctl daemon-reload
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [20 - Lab 03: OOM Killer](./20-Hands-On-Lab-03-OOM-Killer-Analysis.md) | [README](./README.md) | [22 - Lab 05: Port Conflict](./22-Hands-On-Lab-05-Port-Binding-Conflict.md) |
