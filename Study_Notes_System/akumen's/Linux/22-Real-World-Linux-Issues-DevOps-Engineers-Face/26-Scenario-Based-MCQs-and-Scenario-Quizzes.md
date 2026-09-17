# 26 — Scenario-Based MCQs and Diagnostic Quizzes

## Question 1
A shell script fails with error `bash: ./test.sh: /bin/bash^M: bad interpreter`. Which command fixes this issue instantly?

- A) `chmod +x test.sh`
- B) `dos2unix test.sh`
- C) `chown root:root test.sh`
- D) `source test.sh`

**Answer**: **B**
*Explanation*: The `^M` character indicates Windows CRLF (`\r\n`) line endings. `dos2unix` converts CRLF to Unix LF (`\n`).

---

## Question 2
Which `lsof` flag lists open files that have been unlinked from the directory structure but are still consuming disk space?

- A) `lsof -i`
- B) `lsof +L1`
- C) `lsof -p 1`
- D) `lsof -u root`

**Answer**: **B**
*Explanation*: `+L1` instructs `lsof` to list open files with a link count less than 1 (`NLINK == 0`), identifying unlinked open files.

---

## Question 3
What does systemd exit code `203/EXEC` signify when inspecting `systemctl status`?

- A) WorkingDirectory path is invalid.
- B) Service user account does not exist.
- C) Binary path in ExecStart is missing or not executable.
- D) Process timed out during startup.

**Answer**: **C**
*Explanation*: `203/EXEC` indicates that `systemd` failed to execute the binary specified in `ExecStart`.

---

## Question 4
A container exits with code `137`. What is the primary cause?

- A) Application syntax error.
- B) Container killed by kernel OOM-killer due to memory limits.
- C) Docker volume permission error.
- D) Invalid port binding.

**Answer**: **B**
*Explanation*: Exit code `137` represents termination by signal 9 (`SIGKILL`, 128 + 9), triggered by kernel OOM-killer.

---

## Question 5
Which command displays per-core CPU utilization breakdown including user, system, and I/O wait percentages?

- A) `mpstat -P ALL 1`
- B) `uptime`
- C) `free -m`
- D) `df -h`

**Answer**: **A**
*Explanation*: `mpstat -P ALL` provides detailed per-CPU core activity statistics.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [25 - DevOps Interview Q&A](./25-DevOps-Troubleshooting-Interview-Questions.md) | [README](./README.md) | [27 - Incident Response Checklist](./27-Production-Incident-Response-Checklist.md) |
