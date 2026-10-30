# 03 - SSH Pipelining, ControlMaster, & Connection Plugins

In agentless architecture, SSH is the primary transport mechanism connecting the Ansible control node to target servers. However, default naive SSH connections incur enormous operational latency: establishing a fresh TCP connection, executing cryptographic key exchange, and authenticating user credentials for every single task turns a 1-minute playbook into a 15-minute ordeal.

By mastering **SSH Multiplexing (`ControlMaster`)**, **SSH Pipelining**, and specialized **Connection Plugins**, engineering teams can reduce playbook execution times by **60% to 80%**.

---

## ⚡ 1. The SSH Latency Problem: Without Pipelining

In default Ansible operation without pipelining, executing a single task on a remote node requires multiple sequential SSH round-trips:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        DEFAULT NON-PIPELINED SSH EXECUTION                             │
│                                                                                        │
│   Task: ansible.builtin.command: cmd="uptime"                                          │
│                                                                                        │
│   1. SSH Connect #1 ──► Create temporary directory: mkdir -p ~/.ansible/tmp/...        │
│   2. SFTP/SCP Connect #2 ──► Upload packaged Ansiballz Python module zip to remote     │
│   3. SSH Connect #3 ──► Set file permissions: chmod +x ~/.ansible/tmp/.../Ansiballz... │
│   4. SSH Connect #4 ──► Execute module with sudo: /usr/bin/python3 Ansiballz...        │
│   5. SSH Connect #5 ──► Delete temporary directory: rm -rf ~/.ansible/tmp/...          │
│                                                                                        │
│   Total Round-Trips per Task: 4 to 5 separate SSH connections!                         │
│   Over 50ms latency link: 5 connections x 50ms = 250ms delay per task per server.      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 2. SSH Pipelining (`pipelining = True`)

SSH Pipelining is the single most effective performance switch in Ansible. Instead of copying a Python payload to disk via SFTP and executing it in a subsequent SSH command, pipelining **streams the Python module directly into the remote Python interpreter via standard input (`stdin`)**.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        PIPELINED SSH EXECUTION WITH CONTROLMASTER                      │
│                                                                                        │
│   Control Node ═════════════════[Open Multiplexed Socket]═══════════════► Target Node  │
│                                                                                        │
│   1. Single SSH execution stream:                                                      │
│      ssh -O check ... | /usr/bin/python3 -c "exec(sys.stdin.read())"                   │
│                                                                                        │
│   • ZERO SFTP/SCP file transfers                                                       │
│   • ZERO remote temporary files written to /tmp or ~/.ansible                          │
│   • ZERO permission changes (chmod)                                                    │
│   • 1 single SSH operation per task instead of 5!                                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Enabling Pipelining in `ansible.cfg`

```ini
[connection]
pipelining = True
```

### The `requiretty` Obstacle in `/etc/sudoers`

Historically, some enterprise Linux distributions (such as older RHEL/CentOS versions) enabled `Defaults requiretty` inside `/etc/sudoers`. This setting forbids `sudo` from running unless attached to a physical or pseudo-terminal (TTY).

Because SSH pipelining streams data directly through standard input without allocating a pseudo-terminal (`-tt`), running pipelining against a host with `requiretty` produces this immediate error:
```text
fatal: [web1]: FAILED! => {"msg": "sudo: a terminal is required to read the password"}
```

#### Resolution:
Modern security consensus (and modern versions of Ubuntu, Debian, and RHEL 8+) has completely removed `requiretty` because it offers zero genuine security benefits while breaking automation. To fix it, remove or disable `requiretty` in `/etc/sudoers`:

```text
# Edit via visudo on managed nodes:
# Defaults requiretty   <── Comment out or remove this line
```

---

## 🔌 3. SSH Multiplexing (`ControlMaster` & `ControlPersist`)

Even with pipelining, establishing a new SSH session per task incurs TCP three-way handshake and Diffie-Hellman cryptographic exchange overhead. OpenSSH provides **Connection Multiplexing** to reuse a single persistent master SSH socket across all tasks.

### OpenSSH Multiplexing Directives

| Directive | Value | Architectural Behavior |
| :--- | :--- | :--- |
| `ControlMaster` | `auto` | Automatically creates a master connection socket if one does not exist; subsequent connections reuse the existing socket. |
| `ControlPersist` | `60s` / `15m` | Keeps the master SSH socket alive in the background for the specified duration after a task finishes, ready for the next task. |
| `ControlPath` | `~/.ansible/cp/%r@%h:%p` | Filesystem path for the UNIX domain socket representing the active SSH session. |

### Complete Production `ansible.cfg` SSH Configuration

```ini
[defaults]
pipelining = True
forks = 50

[ssh_connection]
# Enable SSH pipelining
pipelining = True

# ControlMaster settings
ssh_args = -C -o ControlMaster=auto -o ControlPersist=15m -o PreferredAuthentications=publickey

# UNIX domain socket location (keep path short to avoid OS socket length limits!)
control_path_dir = ~/.ansible/cp
control_path = %(directory)s/ansible-%%r@%%h:%%p

# Disable StrictHostKeyChecking in dynamic ephemeral cloud environments
# (Enable or maintain known_hosts in strict high-security corporate networks)
# ssh_args = %(ssh_args)s -o StrictHostKeyChecking=accept-new
```

---

## 🌐 4. Beyond SSH: Specialized Connection Plugins

While OpenSSH is the default, Ansible's modular plugin architecture supports native transports for containers, hypervisors, and Windows:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANSIBLE CONNECTION PLUGINS LANDSCAPE                            │
│                                                                                        │
│   ansible.builtin.ssh      ──► Standard Linux/Unix servers (Port 22)                   │
│   ansible.builtin.local    ──► Executes locally on Control Node (Terraform / Cloud APIs│
│   community.docker.docker  ──► Direct container execution via 'docker exec' (No SSH!) │
│   community.k8s.kubectl    ──► Direct Kubernetes pod execution via 'kubectl exec'      │
│   ansible.builtin.winrm    ──► Windows servers via WinRM HTTPS (Port 5986)             │
│   ansible.windows.psrp     ──► Modern Windows PowerShell Remoting Protocol             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 1. `community.docker.docker` (Direct Container Execution)
Bypasses SSH completely and executes Ansible modules directly inside running containers using the local Docker daemon socket:
```ini
[containers]
my_nginx_container ansible_connection=community.docker.docker
```

### 2. `community.kubernetes.kubectl` (Kubernetes Pod Execution)
Executes tasks inside a running Kubernetes container via the Kubernetes API server using `kubectl exec`:
```ini
[pods]
web_pod ansible_connection=community.kubernetes.kubectl ansible_kubectl_pod=web-pod-7df84 ansible_kubectl_namespace=production
```

### 3. `ansible.builtin.local` (Localhost Execution)
Executes directly on the control node without any network transport. Essential for orchestrating cloud resources (AWS EC2, Google Cloud, Azure) or managing local files:
```yaml
- name: Provision Cloud Infrastructure
  hosts: localhost
  connection: local
  gather_facts: false
  tasks:
    - name: Launch AWS EC2 Instance
      amazon.aws.ec2_instance:
        name: web-server-01
        instance_type: t3.medium
        image_id: ami-0c55b159cbfafe1f0
```

---

## 📊 5. Performance Benchmark Comparison

| Configuration Mode | Tasks per Minute | SSH Connections per Task | Average Run Duration (100 Hosts, 20 Tasks) |
| :--- | :--- | :--- | :--- |
| **Default Settings** (No pipelining, default 5 forks, no ControlMaster) | ~20 tasks/min | 5 discrete connections | **22 minutes 40 seconds** |
| **Optimized Multiplexing** (forks=50, ControlMaster=auto, ControlPersist=15m) | ~80 tasks/min | 1 multiplexed socket | **4 minutes 15 seconds** |
| **Full Pipeline Optimization** (forks=50 + Pipelining=True + ControlMaster) | ~350 tasks/min | 1 streamed stdin payload | **58 seconds (23x Speedup!)** |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Parallelism Forks Serial and Batch Execution](./02-Parallelism-Forks-Serial-and-Batch-Execution.md) | [Index](../../../README.md) | [04 - Execution Strategies linear free and host_pinned →](./04-Execution-Strategies-linear-free-and-host_pinned.md) |
