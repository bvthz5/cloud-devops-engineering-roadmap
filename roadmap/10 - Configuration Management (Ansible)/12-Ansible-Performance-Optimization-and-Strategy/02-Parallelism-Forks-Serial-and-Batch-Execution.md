# 02 - Parallelism: Forks, Serial & Throttle

- `forks = 50`: Controls how many hosts Ansible communicates with in parallel (default is 5).
- `serial: "20%"`: Batch size execution across hosts.
- `throttle: 1`: Limit max parallel tasks for specific resource-constrained steps.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Performance Profiling](./01-Ansible-Performance-Bottlenecks-and-Profiling.md) | [README](./README.md) | [03 - SSH Pipelining](./03-SSH-Pipelining-ControlMaster-and-Connection-Plugins.md) |
