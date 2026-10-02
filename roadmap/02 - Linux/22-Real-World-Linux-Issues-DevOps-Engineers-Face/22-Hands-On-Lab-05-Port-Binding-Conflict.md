# 22 — Hands-On Lab 05: Port Binding Conflict

## Lab Objective
Simulate a TCP port conflict by starting a background listener on port 8080, attempt to bind another process to port 8080, identify the conflicting process PID using `ss` and `fuser`, and resolve the conflict.

## Step-by-Step Instructions

### Step 1: Bind Port 8080 in Background
```bash
python3 -m http.server 8080 &
PY_PID=$!
echo "Python web server PID: $PY_PID"
```

### Step 2: Attempt Duplicate Binding (Fails with EADDRINUSE)
```bash
python3 -m http.server 8080
# Output: OSError: [Errno 98] Address already in use
```

### Step 3: Identify Conflicting Process
```bash
# Method A: ss socket inspection
ss -tulpn | grep ':8080'

# Method B: fuser port inspection
fuser 8080/tcp
```

### Step 4: Resolve Port Conflict
```bash
# Terminate process holding port 8080
fuser -k -9 8080/tcp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [21 - Lab 04: Systemd Crash Loop](./21-Hands-On-Lab-04-Systemd-Service-Crash-Loop.md) | [README](./README.md) | [23 - Hands-On Labs 06 to 10](./23-Hands-On-Labs-06-to-10-SSH-DNS-Docker-K8s.md) |
