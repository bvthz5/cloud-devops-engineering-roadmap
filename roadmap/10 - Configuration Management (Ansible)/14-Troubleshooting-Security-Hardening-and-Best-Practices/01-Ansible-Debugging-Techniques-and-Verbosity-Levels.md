# 01 - Ansible Debugging Techniques & Verbosity Levels

## Verbosity Levels
- `-v`: Print task results
- `-vv`: Print task input configuration
- `-vvv`: Print SSH connection details
- `-vvvv`: Maximum debug (includes SSH plugin connection details)

## Interactive Debugger (`debugger: on_failed`)
```yaml
- name: Task with Interactive Debugger
  ansible.builtin.command: /usr/local/bin/flaky_script
  debugger: on_failed
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Dry Run & Diff Modes](./02-Dry-Run-and-Diff-Modes-check-and-diff.md) |
