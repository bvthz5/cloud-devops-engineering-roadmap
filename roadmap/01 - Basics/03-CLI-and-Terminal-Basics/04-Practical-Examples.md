# CLI & Terminal Basics — Practical Commands & Pipelines

## 1. Inspecting Executables & PATH

```bash
# Display PATH directories
echo $PATH

# Locate exact executable binary path
which docker
which python3

# Inspect command type (builtin, alias, binary, or function)
type cd
type ls
type git
```

---

## 2. Redirection & Stream Manipulation

```bash
# Redirect stdout to a file (overwrite)
echo "ENVIRONMENT=production" > app.env

# Append stdout to file
echo "PORT=8080" >> app.env

# Redirect stderr to separate file
ls /nonexistent_folder 2> error.log

# Redirect stdout AND stderr to same log file
python3 script.py > output.log 2>&1
```

---

## 3. Power Pipelines & Filtering

```bash
# Count number of running processes
ps aux | wc -l

# Search active network ports listening for connections
sudo netstat -tulpn | grep LISTEN

# Find and sort top 5 memory-consuming processes
ps aux --sort=-%mem | head -n 6
```

---

## 4. Exit Code Checking & Conditional Chaining

```bash
# Check exit code of previous command ($?)
ls /etc/hosts
echo $?  # Output: 0

ls /invalid_path
echo $?  # Output: 2

# Chain build, test, and deploy (fails fast if build/test fails)
make build && make test && make deploy

# Fallback pattern if directory doesn't exist
cd /var/log/app || mkdir -p /var/log/app
```
