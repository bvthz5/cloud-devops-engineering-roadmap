# 16 — Hands-On Practice Labs: CLI and Terminal Basics

Practical, interactive laboratory exercises designed to reinforce stream redirection, parameter expansions, log parsing pipelines, defensive Bash scripting, and modern CLI tools.

---

## Lab 1: Stream Redirection & Error Separation

### Objective
Separate clean standard output from standard error logs, merge streams, and verify stream redirection mechanics.

### Commands to Execute
```bash
# 1. Create a test directory with mixed files
mkdir -p /tmp/cli_lab && cd /tmp/cli_lab
touch file1.txt file2.txt

# 2. Run a command generating both stdout and stderr
ls -l file1.txt nonexistent.txt

# 3. Capture ONLY errors into error.log
ls -l file1.txt nonexistent.txt 2> error.log
cat error.log

# 4. Capture clean output in stdout.log, errors in stderr.log
ls -l file1.txt nonexistent.txt > stdout.log 2> stderr.log
cat stdout.log
cat stderr.log

# 5. Merge both streams into a single combined.log file
ls -l file1.txt nonexistent.txt > combined.log 2>&1
cat combined.log

# 6. Discard all output completely (silent run)
ls -l file1.txt nonexistent.txt &> /dev/null
echo "Exit code: $?"
```

---

## Lab 2: Building an Nginx Web Server Log Analyzer Pipeline

### Objective
Process raw access log entries to calculate visitor traffic distributions, identify HTTP error spikes, and extract top requested URLs.

### Step 1: Generate Mock Web Server Access Log
```bash
cat << 'EOF' > /tmp/cli_lab/access.log
192.168.1.10 - - [02/Oct/2026:07:00:01 +0000] "GET /api/v1/users HTTP/1.1" 200 452
10.0.0.15 - - [02/Oct/2026:07:00:02 +0000] "POST /api/v1/login HTTP/1.1" 401 128
192.168.1.10 - - [02/Oct/2026:07:00:03 +0000] "GET /api/v1/products HTTP/1.1" 200 1205
172.16.0.4 - - [02/Oct/2026:07:00:04 +0000] "GET /api/v1/checkout HTTP/1.1" 500 89
10.0.0.15 - - [02/Oct/2026:07:00:05 +0000] "GET /api/v1/users HTTP/1.1" 200 452
192.168.1.10 - - [02/Oct/2026:07:00:06 +0000] "GET /api/v1/users HTTP/1.1" 200 452
10.0.0.15 - - [02/Oct/2026:07:00:07 +0000] "POST /api/v1/login HTTP/1.1" 200 310
172.16.0.4 - - [02/Oct/2026:07:00:08 +0000] "GET /api/v1/checkout HTTP/1.1" 502 110
EOF
```

### Step 2: Execute Analytical Pipelines
```bash
# 1. Find Top IP Addresses by Request Count:
awk '{print $1}' /tmp/cli_lab/access.log | sort | uniq -c | sort -nr

# 2. Count Total HTTP Response Status Codes:
awk '{print $9}' /tmp/cli_lab/access.log | sort | uniq -c | sort -nr

# 3. Extract and display only 5xx Server Errors:
grep -E "HTTP/1.1\" 50[0-9]" /tmp/cli_lab/access.log
```

---

## Lab 3: Bash Parameter Expansion Challenges

### Objective
Perform string manipulation directly in Bash without spawning external subprocesses (`sed`, `cut`, `awk`).

### Commands to Execute in Shell
```bash
IMAGE="registry.internal.org:5000/payments/api-gateway:v2.4.1"

# 1. Extract only the tag (strip longest prefix ending in ':')
echo "Tag: ${IMAGE##*:}"
# Output: v2.4.1

# 2. Extract only the repository registry host (strip shortest suffix starting with '/')
echo "Registry: ${IMAGE%%/*}"
# Output: registry.internal.org:5000

# 3. Strip both registry and tag to get the service name
NAME_WITH_TAG="${IMAGE#*/}"
SERVICE_NAME="${NAME_WITH_TAG%:*}"
echo "Service: ${SERVICE_NAME}"
# Output: payments/api-gateway

# 4. Demonstrate Default Fallback
unset ENVIRONMENT
echo "Deploying to: ${ENVIRONMENT:-staging}"
# Output: Deploying to: staging

# 5. String length
echo "Image string character count: ${#IMAGE}"
```

---

## Lab 4: Job Control and Background Execution

### Objective
Suspend, background, resume, and manage terminal jobs.

### Commands to Execute
```bash
# 1. Launch a long-running foreground job
sleep 100

# 2. Press Ctrl + Z to suspend the process
# Terminal outputs: [1]+  Stopped   sleep 100

# 3. View active shell jobs
jobs -l

# 4. Resume the job in the BACKGROUND
bg %1
jobs -l # Shows State: Running

# 5. Bring the job back to the FOREGROUND
fg %1

# 6. Press Ctrl + C to terminate it
```

---

## Lab 5: Writing a Defensive CI/CD Deployment Script

### Objective
Create an automated deployment script demonstrating `set -euo pipefail`, signal trapping, and safe exit handling.

### Step 1: Create the Script
Save to `/tmp/cli_lab/ci_deploy.sh`:
```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

TMP_BUILD=$(mktemp -d -t build_XXXXXX)
cleanup() {
    echo "[CLEANUP] Removing temporary build directory ${TMP_BUILD}"
    rm -rf "${TMP_BUILD}"
}
trap cleanup EXIT ERR

echo "[INFO] Starting CI build process..."
echo "Simulating code compilation in ${TMP_BUILD}..."
echo "Compiled Artifact v1.0.0" > "${TMP_BUILD}/app.bin"

echo "[INFO] Running automated tests..."
# Intentionally test pipefail:
echo "All tests passed" | grep "tests passed"

echo "[SUCCESS] Build and test deployment verified successfully!"
```

### Step 2: Test Execution
```bash
chmod +x /tmp/cli_lab/ci_deploy.sh
/tmp/cli_lab/ci_deploy.sh
```
Observe that the script creates the temp directory, passes validation, and executes the cleanup trap automatically upon exit.

---

## Lab 6: JSON Parsing with `jq`

### Objective
Extract and filter structured data from modern REST APIs and Kubernetes JSON responses.

### Commands to Execute
```bash
# Create mock JSON response
cat << 'EOF' > /tmp/cli_lab/nodes.json
{
  "cluster": "prod-useast1",
  "nodes": [
    {"name": "node-01", "role": "control-plane", "ready": true, "cpu": 16},
    {"name": "node-02", "role": "worker", "ready": true, "cpu": 32},
    {"name": "node-03", "role": "worker", "ready": false, "cpu": 32}
  ]
}
EOF

# 1. Extract cluster name:
jq -r '.cluster' /tmp/cli_lab/nodes.json

# 2. Extract names of all nodes:
jq -r '.nodes[].name' /tmp/cli_lab/nodes.json

# 3. Filter only worker nodes that are NOT ready:
jq -r '.nodes[] | select(.role=="worker" and .ready==false) | .name' /tmp/cli_lab/nodes.json
```

Clean up lab directory:
```bash
rm -rf /tmp/cli_lab
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 15 - Interview QA](./15-Interview-QA.md) | [Index](../../../README.md) | [17 - MCQ →](./17-MCQ.md) |
