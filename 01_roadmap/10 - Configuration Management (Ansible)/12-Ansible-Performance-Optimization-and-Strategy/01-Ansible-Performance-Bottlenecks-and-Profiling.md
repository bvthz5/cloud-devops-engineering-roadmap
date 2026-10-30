# 01 - Ansible Performance Profiling

Enable callback plugins to measure task execution durations:
```ini
[defaults]
callbacks_enabled = timer, profile_tasks, profile_roles
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (11-Testing-Ansible-with-Molecule-and-Lint)](../11-Testing-Ansible-with-Molecule-and-Lint/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Parallelism Forks Serial and Batch Execution →](./02-Parallelism-Forks-Serial-and-Batch-Execution.md) |
