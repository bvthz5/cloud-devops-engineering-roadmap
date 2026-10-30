# Configuration Management (Ansible) Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Idempotency Breakdown: Task Re-Executing Every Run with 'changed: true'

### 🚨 The Production Scenario
A playbook configuring configuration files and services shows `changed=12` on every single execution, even when zero configuration has changed. Handlers trigger continuously, causing service restarts.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Using raw `command` or `shell` modules instead of declarative Ansible modules (like `template`, `copy`, `lineinfile`, `systemd`). The `command` module has no built-in state comparison and unconditionally registers as 'changed' unless explicit guards (`creates`, `removes`, `changed_when`) are configured.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Replace generic `shell`/`command` tasks with native declarative modules.
- Step 2: If `command` or `shell` is unavoidable, add `creates: /path/to/file` or `removes` file checks.
- Step 3: Define explicit `changed_when: false` or `changed_when: result.rc == 0 and 'updated' in result.stdout`.
- Step 4: Use `check_mode: yes` and `--diff` during CI testing to detect non-idempotent tasks.
- Step 5: Verify that consecutive runs of the playbook produce `changed=0`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Prevent repeated execution by adding creates guard to command module
cat <<EOF > task.yml
- name: Run command idempotently
  command: /opt/app/setup.sh
  args:
    creates: /etc/app/initialized.lock
EOF

# Mark query or read-only tasks as changed_when: false to avoid spurious handler triggers
cat <<EOF > task2.yml
- name: Query database status
  command: /usr/local/bin/check_db.sh
  register: db_status
  changed_when: false
EOF

# Audit playbook changes in dry-run mode to inspect diffs without applying
ansible-playbook site.yml --check --diff

# Run Ansible Lint to automatically flag anti-patterns like un-guarded shell modules
ansible-lint site.yml

# Verify that secondary execution reports exactly changed=0
ansible-playbook site.yml | grep -E 'failed=[^0]|changed=[^0]'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Idempotence is the foundational principle of Ansible: running a playbook ten times should leave the system in the exact same state as running it once. Tasks report false changes when engineers use `shell` or `command` without guards. I enforce native declarative modules (`template`, `package`, `systemd`). If shell execution is strictly required, I bind it with `creates: /path/to/lock` or explicit `changed_when` conditions, and integrate `ansible-lint` in CI to fail any PR containing unconstrained shell tasks."

---

## 📌 Scenario 5: Dynamic Inventory Failure on Autoscaled Cloud Instances

### 🚨 The Production Scenario
An Ansible deployment playbook fails to target newly launched EC2 or Azure instances during a traffic surge, or targets terminated instances with SSH timeouts.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Using a static `hosts` file or using dynamic inventory plugins (`amazon.aws.aws_ec2`, `azure.azcollection.azure_rm`) with stale local cache configurations. If dynamic inventory caching (`cache: yes`, `cache_timeout: 3600`) holds instances for 1 hour, terminated instances remain in inventory and newly scaled instances are ignored.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Use official cloud dynamic inventory plugins (`aws_ec2.yml`) instead of legacy Python scripts.
- Step 2: Disable local inventory caching or reduce `cache_timeout` to 60 seconds.
- Step 3: Use strict tag-based filters (e.g. `tag:Environment=production` and `instance-state-name: running`).
- Step 4: Use `keyed_groups` to create dynamic groups based on tags, availability zones, and roles.
- Step 5: Verify live discovery with `ansible-inventory --graph`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect the real-time dynamically generated host tree from AWS
ansible-inventory -i aws_ec2.yml --graph

# Verify active discovered hostnames matching tag filters
ansible-inventory -i aws_ec2.yml --list | jq '._meta.hostvars | keys'

# Force flush of cached inventory data before executing playbook
ansible-playbook -i aws_ec2.yml --flush-cache site.yml

# Configure production dynamic inventory YAML manifest
cat <<EOF > aws_ec2.yml
plugin: amazon.aws.aws_ec2
regions: [us-east-1]
filters:
  instance-state-name: running
  tag:Role: webserver
keyed_groups:
  - key: tags.Environment
    prefix: env
EOF

# Ping all currently running production instances discovered live
ansible env_production -i aws_ec2.yml -m ping

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Static inventories cannot manage autoscaling cloud environments. I implement official cloud dynamic inventory plugins (e.g. `amazon.aws.aws_ec2`). I filter strictly on `instance-state-name: running` and key tags (like `Environment: production`), configure `keyed_groups` to categorize nodes dynamically, and run with `--flush-cache` in deployment pipelines to ensure newly scaled instances are immediately targeted while terminated instances are dropped."

---

## 📌 Scenario 6: Ansible Handler Fails to Trigger because Preceding Task Skips or Fails

### 🚨 The Production Scenario
A configuration template is updated, but Nginx is never restarted because an unrelated downstream task failed, leaving the old server configuration active in memory.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
By default, Ansible handlers are queued and execute at the very end of the play. If any task fails before the end of the play, Ansible halts execution immediately, and all queued handlers are abandoned. Additionally, handlers only run if the task that notified them reported `changed: true`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Use `meta: flush_handlers` immediately after critical configuration tasks to force queued handlers to run immediately.
- Step 2: Configure `force_handlers = True` in `ansible.cfg` or play definition to execute handlers even if downstream tasks fail.
- Step 3: Verify the notifying task actually reports `changed` state.
- Step 4: Check handler naming for exact character and case matches.
- Step 5: Test handler execution under failure scenarios using test plays.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Flush and execute queued handlers immediately without waiting for end of play
cat <<EOF >> deploy.yml
  tasks:
    - name: Update nginx conf
      template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
      notify: Restart Nginx
    - meta: flush_handlers
EOF

# Verify if force_handlers=True is enabled in global Ansible configuration
grep -i 'force_handlers' /etc/ansible/ansible.cfg

# Force execution of triggered handlers even if errors occur downstream
ansible-playbook site.yml --force-handlers

# Inspect verbose logs to confirm handler was successfully registered
ansible-playbook site.yml -v | grep -A 5 'NOTIFIED'

# Verify daemon restarted and re-read newly deployed configuration
systemctl status nginx

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Handlers deferred to the end of a play create operational risks: if task 20 fails, the restart handler for task 2 is discarded, leaving services running mismatched configs. I mitigate this in two ways: 1) inserting `meta: flush_handlers` immediately after critical configuration changes so services restart before proceeding to verification tasks, and 2) setting `force_handlers = True` in `ansible.cfg` so handlers run even if downstream non-critical tasks fail."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Configuration Management (Ansible) Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Configuration Management (Ansible) Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

