# 13 — Real-World DevOps & CI/CD CLI Scenarios

Command-line and shell mechanics power every CI/CD runner, deployment script, and automated cron job. Below are real-world production outage scenarios caused by subtle CLI oversights.

---

## Scenario 1: Silent Pipeline Failure Due to Missing `pipefail`

### Problem Statement
A GitHub Actions CI/CD pipeline deploys a broken Node.js microservice to production. The automated testing step was supposed to block the deployment if unit tests failed. The workflow file contained:
```yaml
run: |
  npm test | tee test-results.log
```
Even though `npm test` failed with 14 failing test assertions and exit code `1`, GitHub Actions marked the step as **Green / Success** and proceeded to production deployment.

### Architecture Analysis
In standard Bash, the exit status of a pipeline (`cmd1 | cmd2`) is **determined strictly by the exit status of the LAST command in the pipeline**.
- `npm test` returned exit code `1` (Failure).
- `tee test-results.log` successfully wrote the output to disk and exited with code `0` (Success).
- Bash evaluated `0` as the final exit code for the entire step, masking the test failure!

```text
[ npm test (Exit: 1) ] ──(pipe)──► [ tee log (Exit: 0) ] ──► Pipeline Exit Code: 0 (PASSED!)
```

### Engineering Solution
Enable **`pipefail`** option in the shell:
```bash
set -o pipefail
npm test | tee test-results.log
```
With `pipefail` active, the return value of a pipeline is the exit status of the **last (rightmost) command to exit with a non-zero status**, or zero if all commands exited successfully.
In GitHub Actions, you can configure this globally across all steps:
```yaml
defaults:
  run:
    shell: bash --noprofile --norc -eo pipefail {0}
```

---

## Scenario 2: Unquoted Variable & Catastrophic `rm -rf`

### Problem Statement
An automated cleanup cron job on a multi-tenant file server executed:
```bash
CLEANUP_TARGET="/var/cache/app"
rm -rf $CLEANUP_TARGET /tmp/junk
```
Due to a typo in a configuration refactor, `CLEANUP_TARGET` was mistakenly set to `" /var/cache/app"` (with a leading space), or evaluated to an empty string. The cleanup script accidentally wiped the entire root filesystem (`rm -rf / ...`), taking down 40 production servers.

### Architecture Analysis
When `$CLEANUP_TARGET` is unquoted:
1. The shell performs **Word Splitting**.
2. If the variable is empty (`""`), the command collapses to:
   ```bash
   rm -rf /tmp/junk
   ```
3. If the variable contains `" /var/cache/app"`, word splitting breaks it into two distinct arguments: `/` and `var/cache/app`.
4. The command becomes:
   ```bash
   rm -rf / var/cache/app /tmp/junk  # Deletes root directory!
   ```

### Engineering Solution
1. **Always Double Quote Variables:** `"$CLEANUP_TARGET"` preserves spaces as a single argument.
2. **Defensive Parameter Expansion with Unset Guard:**
   ```bash
   # Halt script immediately if variable is unset or empty:
   rm -rf "${CLEANUP_TARGET:?Variable CLEANUP_TARGET is unset or empty!}"
   ```
3. **Validate Target Path Before Deleting:**
   ```bash
   if [[ -n "${CLEANUP_TARGET}" && "${CLEANUP_TARGET}" != "/" && -d "${CLEANUP_TARGET}" ]]; then
       rm -rf "${CLEANUP_TARGET}"
   fi
   ```

---

## Scenario 3: Windows CRLF Line Endings Breaking Docker Containers

### Problem Statement
A developer clones a repository on Windows, creates a startup entrypoint script `entrypoint.sh`, commits it via Git, and builds a Docker container. In production Kubernetes on Linux, the pod enters a `CrashLoopBackOff` with the bizarre error:
```text
standard_init_linux.go:228: exec user process caused: no such file or directory
```
However, inspecting the container proves `/entrypoint.sh` definitely exists and has `chmod +x` permissions!

### Architecture Analysis
- Windows uses **CRLF** (`\r\n` - Carriage Return + Line Feed) for line breaks.
- Linux uses **LF** (`\n` - Line Feed) only.
- When Git checks out files on Windows with `core.autocrlf = true`, it converts line endings to CRLF.
- When the Linux kernel executes the shebang `#!/usr/bin/env bash\r`, it searches for a binary named **`bash\r`** (including the carriage return character).
- Because no executable named `bash\r` exists in `/usr/bin/`, the kernel returns `ENOENT (No such file or directory)`.

```text
Shebang on Windows CRLF:
#!/usr/bin/env bash\r
                └── Kernel searches for: "bash\r" ──► ENOENT (No such file or directory)
```

### Engineering Solution
1. **Fix Existing File:**
   ```bash
   sed -i 's/\r$//' entrypoint.sh
   # Or using dos2unix:
   dos2unix entrypoint.sh
   ```
2. **Enforce Repository-Wide LF in `.gitattributes`:**
   Add to the root of the Git repository:
   ```gitattributes
   *.sh text eol=lf
   *.py text eol=lf
   Dockerfile text eol=lf
   ```

---

## Scenario 4: Remote SSH Command Quoting Hell

### Problem Statement
An automation script executes a remote deployment check over SSH:
```bash
DEPLOY_PATH="/opt/app"
ssh user@server "echo Deploying to $DEPLOY_PATH && cd $DEPLOY_PATH && ls -l"
```
During execution, `$DEPLOY_PATH` expanded on the **local machine** rather than the **remote server**, causing the remote script to navigate to an incorrect local path or fail.

### Architecture Analysis
Because double quotes (`"..."`) were used for the SSH command string, the **local shell** evaluated `$DEPLOY_PATH` before passing the string to the `ssh` binary. If the variable had different values locally and remotely, the wrong context was executed.

### Engineering Solution
1. **Single Quotes for Remote Evaluation:**
   ```bash
   # Evaluates $DEPLOY_PATH strictly on the remote server:
   ssh user@server 'DEPLOY_PATH="/opt/app"; cd "$DEPLOY_PATH" && ls -l'
   ```
2. **Streaming a Script via Stdin (Cleaner & Immune to Quoting Issues):**
   ```bash
   ssh user@server 'bash -s' < deploy_script.sh
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Modern CLI Replacements and Productivity Tools](./12-Modern-CLI-Replacements-and-Productivity-Tools.md) | [README](./README.md) | [14 - Troubleshooting](./14-Troubleshooting.md) |
