# 02 - Idempotency Principles & Task Status Lifecycle

The cornerstone of modern configuration management is **Idempotency**. In mathematical terms, an operation is idempotent if applying it multiple times produces the exact same result as applying it once:

$$f(f(x)) = f(x)$$

In Ansible, an idempotent playbook describes the target system's **desired state** rather than a list of procedural commands. Running an Ansible playbook against an unconfigured machine brings it to the desired state (`changed`). Running the exact same playbook a minute later makes **zero changes** (`ok`), verifying that the system already matches the declared policy.

---

## 1. The Anatomy of Task Execution Statuses

When Ansible executes a task against a host, the returned result falls into one of five distinct status categories:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          ANSIBLE TASK STATUS LIFECYCLE                          │
│                                                                                 │
│   STATUS       COLOR    MEANING                          SYSTEM IMPACT          │
│   ──────────── ──────── ──────────────────────────────── ────────────────────── │
│   ok           Green    Desired state already satisfied  Zero changes made      │
│   changed      Yellow   Delta detected and reconciled    Modified state on disk │
│   failed       Red      Task failed or threw exception   Halts play for host    │
│   unreachable  Red      SSH / network connection dropped Host purged from play  │
│   skipped      Cyan     Conditional evaluated to false   Task bypassed          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The Terminal PLAY RECAP Summary
At the conclusion of every playbook run, Ansible prints an aggregated recap:
```text
PLAY RECAP *********************************************************************
web01.prod.internal   : ok=8    changed=2    unreachable=0    failed=0    skipped=1
web02.prod.internal   : ok=10   changed=0    unreachable=0    failed=0    skipped=1
db01.prod.internal    : ok=6    changed=0    unreachable=0    failed=0    skipped=0
```
- `web01`: Received 2 state modifications (e.g., config updated, service restarted).
- `web02`: Already in 100% compliance; zero modifications performed.

---

## 2. Why Shell & Command Modules Break Idempotency (And How to Fix Them)

Declarative modules (like `ansible.builtin.copy`, `ansible.builtin.package`, or `ansible.builtin.user`) check system state before taking action. If a user already exists with UID 1001, the `user` module does nothing and returns `ok`.

However, imperative execution modules—**`ansible.builtin.command`**, **`ansible.builtin.shell`**, and **`ansible.builtin.raw`**—have no built-in knowledge of what their internal scripts do. By default, **they report `changed: true` every single time they run**, destroying idempotency!

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    RESTORING IDEMPOTENCY TO SHELL SCRIPTS                       │
│                                                                                 │
│   ❌ NON-IDEMPOTENT SHELL TASK (Runs & reports changed every apply)             │
│   - name: Initialize Database Cluster                                           │
│     ansible.builtin.command: /usr/bin/initdb -D /var/lib/pgsql/data             │
│                                                                                 │
│   ✅ IDEMPOTENT RESTORATION VIA GUARDS & CHANGED_WHEN                           │
│   - name: Initialize Database Cluster                                           │
│     ansible.builtin.command: /usr/bin/initdb -D /var/lib/pgsql/data             │
│     args:                                                                       │
│       creates: /var/lib/pgsql/data/PG_VERSION   <── Guard: Skips if file exists!│
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Advanced Idempotency Guards: `creates`, `removes`, and `changed_when`

### 1️⃣ Filesystem Guards: `creates` and `removes`

```yaml
# Runs ONLY if the target file DOES NOT exist
- name: Download and unpack custom binary
  ansible.builtin.unarchive:
    src: https://corp.internal/binaries/app-v2.tar.gz
    dest: /opt/app/
    remote_src: yes
    creates: /opt/app/bin/app-v2   # Skips downloading if binary already on disk!

# Runs ONLY if the target file DOES exist
- name: Remove legacy migration lockfile
  ansible.builtin.command: /usr/local/bin/cleanup_locks.sh
  args:
    removes: /var/run/migration.lock # Skips if lockfile has already been deleted
```

---

### 2️⃣ Explicit Status Overrides: `changed_when` and `failed_when`

When querying status or running CLI tools that do not modify state, use `changed_when: false`:

```yaml
# Read-only query: Never report as 'changed'
- name: Query current OpenSSL version
  ansible.builtin.command: openssl version
  register: openssl_output
  changed_when: false # Always reports 'ok' (green), never 'changed' (yellow)

# Complex status detection based on CLI output strings
- name: Seed database schema migration
  ansible.builtin.command: /opt/bin/migrate.sh
  register: migration_result
  # Report 'changed' ONLY if the script printed migration success
  changed_when: "'Applying migration' in migration_result.stdout"
  # Ignore harmless warnings (e.g., schema already up to date)
  failed_when:
    - migration_result.rc != 0
    - "'already up to date' not in migration_result.stderr"
```

---

## 4. The 2-Pass Idempotency Test in CI/CD Pipelines

In automated DevOps testing pipelines (e.g., GitHub Actions, Molecule, GitLab CI), infrastructure roles must be subjected to a **2-Pass Idempotency Verification**:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       CI/CD 2-PASS IDEMPOTENCY PIPELINE                         │
│                                                                                 │
│   PASS 1: INITIAL CONVERGENCE                   PASS 2: IDEMPOTENCY CHECK       │
│   ┌────────────────────────────────┐            ┌─────────────────────────────┐ │
│   │ Run: ansible-playbook site.yml │───────────►│ Run: ansible-playbook site.yml│ │
│   │                                │            │                               │ │
│   │ Expectation:                   │            │ Expectation:                  │ │
│   │ Exit Code = 0                  │            │ Exit Code = 0                 │ │
│   │ changed >= 1 (Installs software)│           │ changed == 0 (ZERO changes!)  │ │
│   └────────────────────────────────┘            └──────────────┬──────────────┘ │
│                                                                │                │
│                                                   ┌────────────┴────────────┐   │
│                                                   │ Did changed > 0 occur?  │   │
│                                                   └────────────┬────────────┘   │
│                                                        YES     │     NO         │
│                                                         ▼      │      ▼         │
│                                                   ┌──────────┐ │ ┌──────────┐   │
│                                                   │ FAIL CI! │ │ │ PASS CI! │   │
│                                                   │ Drift bug│ │ │ Clean    │   │
│                                                   └──────────┘ │ └──────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### GitHub Actions Idempotency Test Script:
```bash
# Pass 1: Convergence
ansible-playbook -i localhost, -c local test.yml

# Pass 2: Idempotency assertion
ansible-playbook -i localhost, -c local test.yml | tee /tmp/ansible_run.log

# Verify that 'changed=0' appears in the PLAY RECAP
grep -q 'changed=0.*failed=0' /tmp/ansible_run.log \
  && (echo "✅ Idempotency Test Passed!"; exit 0) \
  || (echo "❌ Idempotency Test Failed: Tasks re-executed changes!"; exit 1)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Playbook Structure Plays Tasks and YAML Syntax](./01-Playbook-Structure-Plays-Tasks-and-YAML-Syntax.md) | [Index](../../../README.md) | [03 - Handlers and Event Driven Notifications →](./03-Handlers-and-Event-Driven-Notifications.md) |

