# 09 - Python Automation: Interview Questions & Answers

### Q1: Why should an engineer avoid `shell=True` in `subprocess.run()`?
**Answer:** Passing `shell=True` invokes an intermediate system shell (`/bin/sh`). If external user input is concatenated into the command string, an attacker can inject malicious shell commands (Shell Injection). Passing arguments as a list with `shell=False` executes the binary directly via the `execve` system call without shell evaluation.

### Q2: What is the purpose of connection pooling in HTTP client automation?
**Answer:** In microservices, creating a new TCP connection and executing a TLS 1.3 handshake for every single HTTP request adds significant latency overhead. A connection pool (`requests.Session` or `httpx.Client`) reuses existing warm TCP sockets across multiple API calls, reducing latency by up to 5x.

### Q3: How do you parse an extremely large JSON log file (10GB+) in Python without running out of memory?
**Answer:** By streaming the data. If the log is JSON Lines (JSONL), read it line-by-line (`for line in file:`). If it is a single massive JSON array, use an iterative parser like `ijson`, which yields individual objects as they are parsed from the file stream.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
