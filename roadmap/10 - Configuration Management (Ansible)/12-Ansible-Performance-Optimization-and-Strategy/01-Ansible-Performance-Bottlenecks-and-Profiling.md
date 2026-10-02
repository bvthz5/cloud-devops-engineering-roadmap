# 01 - Ansible Performance Profiling

Enable callback plugins to measure task execution durations:
```ini
[defaults]
callbacks_enabled = timer, profile_tasks, profile_roles
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Parallelism & Forks](./02-Parallelism-Forks-Serial-and-Batch-Execution.md) |
