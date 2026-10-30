# 04 - Execution Strategies: `linear` vs `free` vs `host_pinned`

```yaml
- name: Rapid deployment
  hosts: webservers
  strategy: free
  tasks:
    - name: Task 1
      ansible.builtin.apt: name=git state=present
```

- **linear** (default): All hosts complete Task 1 before any host starts Task 2.
- **free**: Hosts run through all tasks as fast as individually possible.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - SSH Pipelining ControlMaster and Connection Plugins](./03-SSH-Pipelining-ControlMaster-and-Connection-Plugins.md) | [Index](../../../README.md) | [05 - Async Tasks and Polling async and poll →](./05-Async-Tasks-and-Polling-async-and-poll.md) |
