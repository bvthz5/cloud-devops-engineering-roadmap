# 10 - Standard I/O Streams, Redirection & Pipelines

Unix architecture centers around three standard I/O streams that allow commands to be chained together into powerful data processing pipelines.

---

## 1. The 3 Standard I/O Streams

Every Linux process is initialized with three default open file descriptors:

| File Descriptor | Name | Stream | Default Source / Target |
|:---:|---|---|---|
| **`0`** | **`stdin`** | Standard Input | Keyboard input stream. |
| **`1`** | **`stdout`** | Standard Output | Terminal screen output. |
| **`2`** | **`stderr`** | Standard Error | Terminal screen error logs. |

---

## 2. Redirection Operators

```bash
# 1. Redirect stdout to a file (OVERWRITES existing content):
$ echo "server_ip=10.0.0.1" > config.env

# 2. Append stdout to a file (PRESERVES existing content):
$ echo "port=8080" >> config.env

# 3. Redirect stderr only to an error log file:
$ ls /nonexistent 2> error.log

# 4. Suppress error messages by routing stderr to the bit bucket:
$ find / -name "app.log" 2> /dev/null

# 5. Redirect BOTH stdout and stderr into a single log file:
$ ./deploy.sh > deploy.log 2>&1
# OR (modern bash shorthand):
$ ./deploy.sh &> deploy.log

# 6. Read stdin from a file instead of the keyboard:
$ mysql -u root -p database_name < schema.sql
```

---

## 3. Pipelines (`|`)

A **pipeline** connects the `stdout` (FD 1) of the preceding command directly to the `stdin` (FD 0) of the succeeding command in RAM, without writing intermediate temporary files to disk.

```bash
# Filter active processes for Nginx and count running instances:
$ ps aux | grep nginx | grep -v grep | wc -l

# Find top 5 heaviest disk users in /var/log:
$ du -sh /var/log/* | sort -hr | head -n 5
```

---

## 4. `tee` — Stream Splitting & Privilege Elevation

The `tee` utility acts as a T-pipe fitting: it writes `stdout` to the terminal screen **and** saves it to one or more files simultaneously.

```bash
# Stream output to screen AND save to log file:
$ ./build.sh | tee build.log

# Append to log file instead of overwriting:
$ ./build.sh | tee -a build.log
```

### The Sudo Redirection Solution:
A common error when editing system files in `/proc` or `/etc`:
```bash
# FAILS! Sudo applies to 'echo', but the redirection '>' runs under your unprivileged user shell:
$ sudo echo 1 > /proc/sys/net/ipv4/ip_forward
bash: /proc/sys/net/ipv4/ip_forward: Permission denied

# CORRECT SOLUTION using tee:
$ echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward > /dev/null
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Vi and Vim](./09-Vi-and-Vim.md) | [README](./README.md) | [11 - All Flags Cheat Sheet](./11-All-Flags-Cheat-Sheet.md) |
