# 02 - Parallelism: Forks, Serial, & Batch Execution Strategies

By default, Ansible operates with extremely conservative concurrency settings designed to run safely on minimal laptops without exhausting CPU or memory. In enterprise production environments managing hundreds or thousands of nodes, these defaults create massive bottlenecks: a 10-second task across 500 servers will take nearly an hour if executed in waves of 5.

To achieve lightning-fast automation and execute safe, zero-downtime rolling upgrades, operators must master Ansible's concurrency mechanics: **Forks**, **Serial Batches**, **Max Fail Percentages**, and **Task Throttling**.

---

## ⚡ 1. The Concurrency Engine: The `forks` Parameter

Ansible achieves parallelism using Python's process-based multiprocessing model. Each fork is an independent Python worker process spawned on the control node to handle SSH communication, module packaging, and return payload parsing for a single managed node.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANSIBLE MULTIPROCESSING FORK POOL                               │
│                                                                                        │
│   ANSIBLE PLAYBOOK CONTROLLER (Main Process)                                           │
│   Hosts Target: 100 webservers | forks = 20                                            │
│                                                                                        │
│   Worker Pool (20 Concurrent Python Processes)                                         │
│   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐        ┌────────┐                        │
│   │ Fork 1 │ │ Fork 2 │ │ Fork 3 │ │ Fork 4 │ ...    │ Fork 20│                        │
│   └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘        └───┬────┘                        │
│       │          │          │          │                 │                             │
│       ▼          ▼          ▼          ▼                 ▼                             │
│     Host 1     Host 2     Host 3     Host 4           Host 20  ◄── Batch 1 (Parallel)  │
│     Host 21    Host 22    Host 23    Host 24          Host 40  ◄── Batch 2 (Parallel)  │
│     ...        ...        ...        ...              Host 100 ◄── Batch 5 (Parallel)  │
│                                                                                        │
│   Total Waves: 100 hosts / 20 forks = 5 waves (instead of 100 waves with default 5!)   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Hardware Capacity Planning Formula

Each Python fork consumes approximately **40MB to 75MB of RAM** and requires CPU time for SSH encryption handshakes. Sizing your forks requires adhering to the control node resource formula:

$$\text{Forks} \le \min\left( \frac{\text{Available RAM} - 1024\text{MB}}{50\text{MB}}, \; \text{CPU Cores} \times 10 \right)$$

- On an 8-core, 16GB RAM control node: `forks = 50` or `forks = 100` is typically ideal.
- Scaling beyond 200 forks often causes SSH socket exhaustion or CPU thrashing on the control node unless SSH connection multiplexing is active.

### Configuring Forks
```ini
# In ansible.cfg
[defaults]
forks = 50
```

Or via CLI flag for a single run:
```bash
ansible-playbook -i production.ini site.yml -f 50
```

---

## 🔄 2. Zero-Downtime Rolling Updates with `serial`

While `forks` controls how many machines Ansible communicates with simultaneously within a task, `serial` defines the **batch size of the play itself**. 

Without `serial`, Ansible runs Task 1 across all 500 hosts, then Task 2 across all 500 hosts. If Task 2 restarts Nginx, **all 500 webservers restart at the same second**, causing a catastrophic production outage!

The `serial` keyword breaks the host pool into sequential batches. A batch executes the entire play from start to finish—including all handlers—before Ansible advances to the next batch.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SERIAL BATCH ROLLING DEPLOYMENT FLOW                            │
│                                                                                        │
│   serial: "25%" (100 Web Servers Total -> 4 Batches of 25)                            │
│                                                                                        │
│   BATCH 1 (Hosts 1 - 25)                                                               │
│   [De-register ALB] ──► [Upgrade App] ──► [Restart Service] ──► [Register ALB]         │
│   (Success! 75% of fleet was online serving traffic)                                   │
│           │                                                                            │
│           ▼                                                                            │
│   BATCH 2 (Hosts 26 - 50)                                                              │
│   [De-register ALB] ──► [Upgrade App] ──► [Restart Service] ──► [Register ALB]         │
│           │                                                                            │
│           ▼                                                                            │
│   BATCH 3 & 4 (Repeat until 100% updated with zero downtime!)                          │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### `serial` Configuration Patterns

```yaml
# Pattern A: Fixed Integer
serial: 5                # Exactly 5 hosts at a time

# Pattern B: Percentage
serial: "20%"            # 20% of matching inventory per wave

# Pattern C: Phased / Canary Deployment Array (Enterprise Standard)
serial:
  - 1                    # Wave 1: Deploy to a single canary node first
  - 5                    # Wave 2: If canary succeeds, test on 5 nodes
  - "20%"                # Wave 3: Scale out to 20%
  - "100%"               # Wave 4: Deploy remaining hosts
```

---

## 🛡️ 3. Failure Boundaries: `max_fail_percentage`

If an application bundle contains a fatal bug that crashes the process on startup, a rolling update might sequentially crash Batch 1, then Batch 2, then Batch 3, destroying the entire cluster.

`max_fail_percentage` sets a strict abort threshold. If the percentage of failed hosts in the current batch exceeds this limit, Ansible aborts the entire playbook immediately, preserving the remaining healthy nodes:

```yaml
- name: Deploy Production Web App
  hosts: webservers
  serial: "20%"
  max_fail_percentage: 10   # Abort immediately if >10% of nodes in batch fail!
  tasks:
    - name: Pull new container
      ...
```

---

## 🚦 4. Task-Level Concurrency Limiting with `throttle`

While the play may operate with `forks = 50` or `serial = 25%`, certain individual tasks must never run on multiple nodes concurrently to avoid resource contention (e.g., database schema migrations, querying a rate-limited license server, or acquiring an NFS lock).

The `throttle` keyword restricts execution concurrency for a specific task without altering the global `forks` or `serial` settings:

```yaml
- name: Run Database Migration
  ansible.builtin.command:
    cmd: /opt/app/bin/migrate.sh
  throttle: 1   # Strict serialization: only 1 node runs this task at any time!
  run_once: false
```

---

## 🎯 5. Production Playbook: Zero-Downtime Rolling Update

```yaml
---
- name: Zero-Downtime Rolling Upgrade of Production Fleet
  hosts: webservers
  become: yes
  serial:
    - 1        # Phase 1: Canary node
    - "25%"    # Phase 2: Quarter of cluster
    - "100%"   # Phase 3: Remainder of cluster
  max_fail_percentage: 0   # Zero tolerance: abort on any failure

  tasks:
    # 1. Gracefully remove node from load balancer pool
    - name: De-register node from HAProxy
      community.general.haproxy:
        state: disabled
        host: "{{ ansible_facts['hostname'] }}"
        backend: web_pool
      delegate_to: lb01.internal

    - name: Allow existing in-flight connections to drain
      ansible.builtin.pause:
        seconds: 15

    # 2. Upgrade application software
    - name: Deploy latest application binary
      ansible.builtin.copy:
        src: /opt/releases/app-v2.5.1
        dest: /usr/local/bin/app
        mode: '0755'
      notify: Restart Application

    # Flush handlers immediately so service restarts before re-joining pool
    - name: Apply service restart
      ansible.builtin.meta: flush_handlers

    # 3. Health check verification
    - name: Verify application endpoint returns HTTP 200
      ansible.builtin.uri:
        url: "http://127.0.0.1:8080/healthz"
        status_code: 200
      retries: 5
      delay: 3

    # 4. Re-enable node in load balancer
    - name: Re-enable node in HAProxy
      community.general.haproxy:
        state: enabled
        host: "{{ ansible_facts['hostname'] }}"
        backend: web_pool
      delegate_to: lb01.internal

  handlers:
    - name: Restart Application
      ansible.builtin.systemd:
        name: myapp
        state: restarted
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ansible Performance Bottlenecks and Profiling](./01-Ansible-Performance-Bottlenecks-and-Profiling.md) | [Index](../../../README.md) | [03 - SSH Pipelining ControlMaster and Connection Plugins →](./03-SSH-Pipelining-ControlMaster-and-Connection-Plugins.md) |
