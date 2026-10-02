# 03 - SSH Pipelining & ControlMaster

SSH Pipelining reduces SSH operations by executing Python modules directly over stdin without copying temporary files over SFTP/SCP.

In `ansible.cfg`:
```ini
[ssh_connection]
pipelining = True
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Parallelism Forks Serial and Batch Execution](./02-Parallelism-Forks-Serial-and-Batch-Execution.md) | [Index](../../../README.md) | [04 - Execution Strategies linear free and host_pinned →](./04-Execution-Strategies-linear-free-and-host_pinned.md) |
