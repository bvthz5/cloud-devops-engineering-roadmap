# 02 - Subprocess Execution and System Management

## 1. Safe Process Execution with `subprocess.run`

Legacy functions like `os.system()` and `os.popen()` are vulnerable to shell injection attacks and should never be used.
Always use **`subprocess.run()`** with arguments passed as a list:

```python
import subprocess
import sys

def execute_command(cmd: list[str]) -> str:
    try:
        result = subprocess.run(
            cmd,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            text=True,
            check=True,      # Raises CalledProcessError if exit code != 0
            timeout=30       # Prevents script hanging indefinitely
        )
        return result.stdout.strip()
    except subprocess.CalledProcessError as e:
        print(f"ERROR: Command '{' '.join(cmd)}' failed with exit code {e.returncode}: {e.stderr}", file=sys.stderr)
        raise
    except subprocess.TimeoutExpired:
        print(f"ERROR: Command '{' '.join(cmd)}' timed out after 30 seconds!", file=sys.stderr)
        raise

# Example: Run kubectl command safely
output = execute_command(["kubectl", "get", "nodes", "-o", "json"])
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Python Foundations](./01-Python-DevOps-Foundations-and-Environment.md) | [README](./README.md) | [03 - CLI Tools](./03-Building-Production-CLI-Tools-argparse-and-click.md) |
