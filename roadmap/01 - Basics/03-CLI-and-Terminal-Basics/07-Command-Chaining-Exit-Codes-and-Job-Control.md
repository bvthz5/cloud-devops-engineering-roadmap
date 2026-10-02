# 07 — Command Chaining, Exit Codes, and Job Control

---

## 1. Command Chaining Operators

In shell scripting and CLI operations, multiple commands can be chained together on a single line using control operators:

```text
Chaining Operators:
├── Sequential ( ; )    : Runs B regardless of A's success or failure
├── Logical AND ( && )  : Runs B ONLY IF A succeeds (Exit Code 0)
└── Logical OR ( || )   : Runs B ONLY IF A fails (Exit Code != 0)
```

### Detailed Operator Comparison

| Operator | Syntax | Description | Example |
| :--- | :--- | :--- | :--- |
| **Semicolon (`;`)** | `cmd1 ; cmd2` | Sequential execution. `cmd2` executes after `cmd1` finishes, regardless of exit status. | `git add . ; git commit -m "update" ; git push` |
| **Logical AND (`&&`)**| `cmd1 && cmd2`| Conditional execution. `cmd2` runs **only if `cmd1` succeeds** (exit code 0). | `mvn test && docker build -t app .` |
| **Logical OR (`||`)** | `cmd1 \|\| cmd2`| Fallback execution. `cmd2` runs **only if `cmd1` fails** (exit code != 0). | `ping -c 1 8.8.8.8 \|\| echo "Network Down"` |
| **Grouped Block (`{}`)**| `{ cmd1; cmd2; }`| Executes group in current shell context. | `mkdir -p /app && { cd /app; npm install; }` |

> **Production Rule:** Never use `;` for multi-stage deployment commands. If step 1 fails (e.g., compile error), step 2 (deploy) will execute anyway and push broken code! Always use **`&&`**.

---

## 2. Exit Status and Standard Linux Exit Codes

Every command that executes in Linux returns an integer **Exit Code** (between `0` and `255`) to the operating system upon termination.
The exit status of the most recently executed command is stored in the special shell parameter **`$?`**.

```bash
ls /etc/hosts
echo $?
# Output: 0 (Success)

ls /nonexistent_file
echo $?
# Output: 2 (Error)
```

### Standard Linux Exit Codes Reference Table

| Exit Code | Meaning | Cause & Kubernetes / DevOps Relevance |
| :--- | :--- | :--- |
| **`0`** | **Success** | Program completed successfully without errors. |
| **`1`** | **General Error** | Catch-all for application errors (e.g., divide by zero, missing file). |
| **`2`** | **Misuse of Shell Built-in** | Syntax error in built-in command (e.g., missing parameter in `exit`). |
| **`126`** | **Command Cannot Execute** | File was found, but lacks executable permissions (`chmod +x` required). |
| **`127`** | **Command Not Found** | Binary is missing from system or not in `$PATH`. |
| **`128`** | **Invalid Exit Argument** | Exit called with non-integer value. |
| **`128 + N`**| **Terminated by Signal N** | The process was killed by Linux Signal number `N`. |
| **`130`** | **Terminated by SIGINT (128+2)**| User pressed `Ctrl+C` in terminal. |
| **`137`** | **Terminated by SIGKILL (128+9)**| **Out-Of-Memory (OOM)!** Pod exceeded memory limit or killed by kernel. |
| **`139`** | **Terminated by SIGSEGV (128+11)**| Segmentation fault (memory corruption / null-pointer dereference). |
| **`143`** | **Terminated by SIGTERM (128+15)**| Graceful eviction (Kubernetes scaled down the deployment). |

---

## 3. Foreground vs Background Processes

- **Foreground Process:** Runs actively in your terminal. It owns the terminal window and standard input (`stdin`). You cannot type further commands until it finishes (e.g., running `tail -f /var/log/syslog`).
- **Background Process:** Runs asynchronously detached from terminal input. The shell prompt returns immediately, allowing you to run other commands while the background job executes.

### Running in Background with `&` and `nohup`
```bash
# 1. Run command in background (appends & at end)
python3 backup_script.py &
# Output: [1] 4512  (Job number 1, Process ID 4512)

# 2. Run background job immune to terminal hangups (nohup)
# Prevents process from dying when SSH connection disconnects:
nohup python3 backup_script.py > backup.log 2>&1 &
```

---

## 4. Job Control: Managing Terminal Tasks

The shell provides a built-in **Job Control** subsystem to suspend, resume, and manage multiple programs in a single terminal session.

```text
                  [ Running Foreground Process ]
                                │
                    Press Ctrl+Z (Sends SIGSTOP)
                                │
                                ▼
                       [ Suspended Job ]
                                │
              ┌─────────────────┴─────────────────┐
              ▼                                   ▼
        Run 'bg %1'                         Run 'fg %1'
  Resumes in BACKGROUND               Brings back to FOREGROUND
```

### Essential Job Control Commands:

| Command | Action |
| :--- | :--- |
| **`Ctrl+C`** | Sends `SIGINT` (Signal 2) to terminate the active foreground process. |
| **`Ctrl+Z`** | Sends `SIGSTOP` (Signal 19) to suspend (pause) the active foreground process. |
| **`jobs`** | Lists all background and suspended jobs owned by the current shell. |
| **`jobs -l`** | Lists jobs along with their corresponding operating system Process IDs (PIDs). |
| **`fg %1`** | Brings job number 1 back into the active foreground. |
| **`bg %1`** | Resumes suspended job number 1 running in the background. |
| **`disown -h %1`**| Removes job 1 from the shell's tracking table so it will not receive `SIGHUP` when you close the terminal. |
