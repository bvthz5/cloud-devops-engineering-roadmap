# 07 - Beginning & Ending Stream Utilities: `head` and `tail`

When dealing with multi-gigabyte log files and tabular datasets, inspect slices of the beginning or ending lines without loading the full file.

---

## 1. `head` — Output the First Part of Files

By default, `head` prints the first **10 lines** of the specified file.

```bash
# Print default first 10 lines:
$ head /etc/passwd

# Print first 25 lines:
$ head -n 25 /var/log/syslog

# Print first 64 bytes of a binary file:
$ head -c 64 /dev/urandom
```

### Pro Tip: Exclude the Trailing Lines
Using a negative integer (`-n -K`) prints all lines from the start **except the last K lines**:
```bash
# Print everything EXCEPT the last 5 lines of the file:
$ head -n -5 report.txt
```

---

## 2. `tail` — Output the Last Part of Files

By default, `tail` prints the last **10 lines** of the specified file.

```bash
# Print default last 10 lines:
$ tail /var/log/nginx/error.log

# Print the last 100 lines:
$ tail -n 100 /var/log/nginx/error.log
```

### Pro Tip: Skip Headers (`-n +K`)
Using a plus sign (`-n +K`) instructs `tail` to start printing from line K through the end of the file:
```bash
# Skip the CSV header row (line 1) and output all data rows:
$ tail -n +2 metrics.csv
```

---

## 3. Real-Time Log Streaming: `tail -f` vs. `tail -F`

In DevOps production monitoring, streaming live logs as new events are appended is a core daily workflow.

```bash
# Follow live log updates in real time:
$ tail -f /var/log/nginx/access.log
# (Press Ctrl+C to stop following)
```

### The Production Difference: `-f` vs. `-F`

```
┌───────────────────────────────────────┬───────────────────────────────────────┐
│              `tail -f`                │              `tail -F`                │
├───────────────────────────────────────┼───────────────────────────────────────┤
│ • Follows the open **File Descriptor**│ • Follows the **File Name** (retry).  │
│ • When `logrotate` rotates the log:   │ • When `logrotate` rotates the log:   │
│   The file is renamed to `app.log.1`. │   `-F` notices the old file was       │
│   `tail -f` remains stuck watching    │   renamed and automatically reopens   │
│   the inactive old file!              │   the newly created `app.log` file!   │
└───────────────────────────────────────┴───────────────────────────────────────┘
```

> [!TIP]
> **Golden DevOps Rule:** Always use **`tail -F`** (capital F) when monitoring production application logs subject to automatic log rotation.

---

## 4. Extracting a Precise Line Slice (Combining `head` and `tail`)

To extract lines 20 through 30 of a file:

```bash
# Method 1: Pipe head into tail:
$ head -n 30 file.txt | tail -n 11

# Method 2 (Direct with sed):
$ sed -n '20,30p' file.txt
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Viewing cat tac less more](./06-Viewing-cat-tac-less-more.md) | [README](./README.md) | [08 - Nano](./08-Nano.md) |
