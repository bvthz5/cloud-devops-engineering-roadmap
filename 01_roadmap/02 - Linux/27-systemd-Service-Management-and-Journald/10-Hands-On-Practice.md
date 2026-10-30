# 10 — Hands-On Practice Labs: systemd

Practical labs for mastering service administration.

---

## Lab 1: Deploying a Custom Hardened Python HTTP Service

### Objective
Write a Python web service, package it into a systemd service unit, enforce sandboxing, and verify automatic self-healing.

1. Create a script `/usr/local/bin/dummy_app.py`:
   ```bash
   sudo bash -c 'cat << "EOF" > /usr/local/bin/dummy_app.py
   import http.server, socketserver
   PORT = 8085
   Handler = http.server.SimpleHTTPRequestHandler
   with socketserver.TCPServer(("", PORT), Handler) as httpd:
       print("Serving at port", PORT)
       httpd.serve_forever()
   EOF'
   sudo chmod +x /usr/local/bin/dummy_app.py
   ```

2. Create `/etc/systemd/system/dummy-app.service`:
   ```ini
   [Unit]
   Description=Dummy Python HTTP Server
   After=network.target

   [Service]
   Type=exec
   ExecStart=/usr/bin/python3 /usr/local/bin/dummy_app.py
   Restart=always
   RestartSec=3s
   NoNewPrivileges=true
   ProtectSystem=full
   PrivateTmp=true

   [Install]
   WantedBy=multi-user.target
   ```

3. Enable, start, and verify self-healing:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable --now dummy-app.service
   systemctl status dummy-app.service

   # Kill the process forcefully and observe systemd restart it in 3 seconds!
   sudo killall -9 python3
   sleep 4
   systemctl status dummy-app.service
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
