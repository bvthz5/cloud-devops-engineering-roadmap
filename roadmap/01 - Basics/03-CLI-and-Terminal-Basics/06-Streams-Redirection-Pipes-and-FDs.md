# 06 — Streams, Redirection, Pipelines, and File Descriptors

---

## 1. The Standard POSIX I/O Streams

Every program executing in a Unix-like operating system is initialized by default with three open data streams represented by numeric **File Descriptors (FD)**:

```text
                 +───────────────────────────────+
                 |       RUNNING PROCESS         |
                 +───────────────────────────────+
                   ▲             │             │
        FD 0: stdin│  FD 1:stdout│  FD 2:stderr│
                   │             │             ▼
    Keyboard / File│             ▼        Terminal Screen / Log
                   │      Terminal Screen
                   │      (Buffered)
```

| Stream Name | File Descriptor | Purpose | Default Device |
| :--- | :--- | :--- | :--- |
| **`stdin`** | **`0`** | Standard Input: Stream where program reads text/binary input | Keyboard / Terminal (`/dev/pts/X`) |
| **`stdout`**| **`1`** | Standard Output: Stream where program writes normal output | Terminal Display Screen |
| **`stderr`**| **`2`** | Standard Error: Stream for diagnostics, errors, and traces | Terminal Display Screen (Unbuffered) |

---

## 2. Output Redirection (`>`, `>>`)

By default, `stdout` displays directly on your screen. Redirection instructs the shell to reconnect a file descriptor to a concrete file or device.

```text
Command ──[ stdout (FD 1) ]──► [ > File ]  (Truncate & Overwrite)
Command ──[ stdout (FD 1) ]──► [ >> File ] (Append to End of File)
```

```bash
# 1. Overwrite file with command output (truncates existing contents)
echo "apiVersion: v1" > pod.yaml

# 2. Append to file without erasing existing contents
echo "kind: Pod" >> pod.yaml
```

> **Safety Tip (`noclobber`):** To prevent accidentally overwriting important files with `>`, enable `set -o noclobber`. The shell will disallow overwriting unless explicitly forced with `>|`.

---

## 3. Error Redirection (`2>`, `2>>`)

Because `stdout` (FD 1) and `stderr` (FD 2) are separate streams, standard output redirection (`>`) does not capture error messages.

```bash
# Attempt to list non-existent file; errors still appear on screen!
ls /nonexistent > output.txt

# Capture ONLY errors to a file:
ls /nonexistent 2> error.log

# Separate clean output from errors cleanly:
find / -name "*.conf" 1> results.txt 2> permission_denied.log
```

---

## 4. Merging Streams: `2>&1` vs `&>`

In cloud automation, you almost always want to capture both normal output and errors in the same log file in chronological order.

```text
Command
  ├── stdout (1) ──► log.txt
  └── stderr (2) ──► Redirected into stream 1 ──► log.txt
```

### The Syntax Explained:
```bash
# POSIX Standard Method (Recommended):
./deploy.sh > deploy.log 2>&1

# Modern Bash Shorthand:
./deploy.sh &> deploy.log
```

### Why Order Matters in `> log 2>&1`
- **Correct:** `command > file.log 2>&1`  
  *Step 1:* Reconnects FD 1 (`stdout`) to `file.log`.  
  *Step 2:* Reconnects FD 2 (`stderr`) to whatever FD 1 is currently pointing at (`file.log`). Both write to the file.
- **Incorrect:** `command 2>&1 > file.log`  
  *Step 1:* Reconnects FD 2 (`stderr`) to current FD 1 (the terminal screen).  
  *Step 2:* Reconnects FD 1 to `file.log`. Result: errors still print to the screen!

---

## 5. Discarding Output: The Black Hole (`/dev/null`)

`/dev/null` is a special virtual character device that instantly discards all data written to it:

```bash
# Discard standard output, keep errors visible:
command > /dev/null

# Discard errors only, keep normal output:
command 2> /dev/null

# Complete silence: Discard both output and errors (Quiet / Silent mode)
command > /dev/null 2>&1
# Or:
command &> /dev/null
```

---

## 6. Input Redirection: `<`

Feeds the contents of a file into a program's `stdin` (FD 0):

```bash
# Pass SQL script to MySQL client via stdin
mysql -u root -p production_db < backup.sql

# Count lines of a file without printing filename in output
wc -l < /var/log/nginx/access.log
```

---

## 7. Here-Documents (`<<EOF`) and Here-Strings (`<<<`)

### Here-Document (`<<EOF`)
Allows passing multi-line blocks of text directly into a command's standard input within a script without creating temporary files:

```bash
cat <<EOF > /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```
- **Disabling Variable Expansion:** Wrap the delimiter in quotes (`<<'EOF'`) to treat the block as 100% literal text without evaluating `$VAR` or `$()`.

### Here-String (`<<<`)
Feeds a single variable or string directly into `stdin`:
```bash
# Decode base64 string using here-string
base64 --decode <<< "SGVsbG8gV29ybGQK"
```

---

## 8. Pipelines (`|`): The Soul of Unix

A **Pipe** connects the standard output (`stdout` - FD 1) of the command on the left directly to the standard input (`stdin` - FD 0) of the command on the right.

```text
[ cat access.log ] ──(stdout)──► [ PIPE ] ──(stdin)──► [ grep "500" ] ──(stdout)──► [ wc -l ]
```

```bash
# Pipeline to find top 5 IP addresses hitting your web server
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -n 5
```

### The `tee` Command: Splitting Pipelines
Pipes stream data directly to the next command without writing to disk. **`tee`** acts like a physical T-splitter pipe, writing data simultaneously to a file AND passing it down the pipeline to `stdout`:

```bash
# Write to root-owned file while preserving terminal view:
echo "deb https://download.docker.com/linux/ubuntu ..." | sudo tee /etc/apt/sources.list.d/docker.list
```
