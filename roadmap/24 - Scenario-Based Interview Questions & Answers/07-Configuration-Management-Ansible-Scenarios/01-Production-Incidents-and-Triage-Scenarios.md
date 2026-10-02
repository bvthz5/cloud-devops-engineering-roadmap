# Configuration Management (Ansible) Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Ansible Playbook Execution Extremely Slow Across 1,000 Nodes

### 🚨 The Production Scenario
Running an Ansible playbook updating packages and configuring Nginx takes 2 hours across a fleet of 1,000 production servers.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
By default, Ansible runs sequentially in small batches (`forks = 5`), establishes full SSH handshakes for every task, and gathers massive system facts on every host. For 1,000 nodes, 5 forks means 200 sequential waves. Additionally, lack of SSH multiplexing and pipelining forces Ansible to transfer temporary python files via SFTP/SCP for every single task execution.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Increase worker concurrency in `ansible.cfg` from default 5 to `forks = 50` or `100`.
- Step 2: Enable SSH Pipelining (`pipelining = True`) to execute Python code in-memory via stdin without disk transfer.
- Step 3: Enable SSH ControlMaster multiplexing (`ControlPersist=60m`) to reuse persistent SSH sockets.
- Step 4: Disable fact gathering (`gather_facts: false`) or configure a Redis/JSON fact cache (`fact_caching = jsonfile`).
- Step 5: Integrate Mitogen for Ansible plugin to accelerate execution by up to 7x.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Configure high-concurrency, pipelining, and SSH connection caching in ansible.cfg
cat <<EOF >> ansible.cfg
[defaults]
forks = 50
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts
[ssh_connection]
pipelining = True
ssh_args = -o ControlMaster=auto -o ControlPersist=60m
EOF

# Execute playbook with 100 parallel worker forks
ansible-playbook -i hosts site.yml -f 100

# Accelerate debugging by jumping directly to specific task
ansible-playbook site.yml --start-at-task='Configure Nginx'

# Benchmark total execution duration improvements
time ansible-playbook site.yml

# Verify connectivity across all nodes concurrently
ansible all -m ping -f 100

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Ansible's default configuration is tuned for safety on small clusters (5 forks, pipelining off, synchronous fact gathering). On a 1,000-node fleet, this creates hours of latency. I optimize this by setting `forks = 50`, enabling `pipelining = True` to run Python directly over SSH stdin without SFTP round-trips, configuring SSH `ControlPersist=60m` to keep sockets alive, and enabling smart fact caching with Redis or local JSON. This drops playbook runtimes from 2 hours to under 8 minutes."

---

## 📌 Scenario 2: Playbook Failing Mid-Run Leaving Production Nodes in an Inconsistent State

### 🚨 The Production Scenario
An Ansible deployment playbook patching an application crashes on task 7 of 10 across 40 nodes due to a network glitch. Half the servers run the new code, while the other half run the old code with mixed configurations.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard Ansible executes tasks top-to-bottom across hosts in a single batch. If a task fails on a subset of hosts, those hosts halt while others continue. Without rollback handlers or failure blocks, nodes are left in a partially applied, broken state.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Wrap critical deployment operations in Ansible `block / rescue` structures to define automated rollback tasks.
- Step 2: Use `serial: 20%` or `max_fail_percentage: 0%` to perform phased canary rollouts across nodes.
- Step 3: Ensure all tasks are strictly idempotent so re-running the playbook is safe.
- Step 4: Execute `ansible-playbook --limit @site.retry` to re-run only the failed nodes once connectivity is restored.
- Step 5: Use pre-tasks and post-tasks to remove nodes from load balancers before changes and re-enable after health checks pass.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Implement canary batching (serial: 20%) and automated rollback blocks
cat <<EOF > deploy.yml
- hosts: web
  serial: "20%"
  max_fail_percentage: 0%
  tasks:
    - block:
        - name: Deploy application
          git: repo=... dest=...
      rescue:
        - name: Rollback to previous tag
          git: repo=... version=v1.2.0
EOF

# Re-run playbook specifically targeting the nodes that failed in previous run
ansible-playbook -i hosts site.yml --limit @site.retry

# Dry-run playbook in check mode to inspect proposed diffs before applying
ansible-playbook site.yml --check --diff

# Execute playbook interactively step-by-step for safe manual inspection
ansible-playbook site.yml --step

# Visualize inventory groupings and canary stages
ansible-inventory --graph

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "To prevent partial failure states in production, I implement three enterprise patterns: 1) `serial: 20%` so changes roll out to one small batch at a time, halting before touching the rest if any node fails, 2) `block / rescue` blocks where any task exception in the block triggers the rescue section to roll back configuration and restart the previous stable service, and 3) strict task idempotence so rerunning `--limit @site.retry` cleanly converges the cluster."

---

## 📌 Scenario 3: Ansible Vault Decryption Failure in Automated CI/CD Pipelines

### 🚨 The Production Scenario
An automated GitLab CI runner executing an Ansible playbook fails with `ERROR! Attempting to decrypt but no vault secrets found` or `Vault password not provided`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Sensitive files or variables are encrypted with Ansible Vault. In automated CI/CD environments, no interactive terminal exists to prompt for a password. If the vault password file or environment variable is missing, misnamed, or has incorrect permissions, Ansible immediately halts.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Store the Vault password in CI/CD masked protected variables (e.g. `ANSIBLE_VAULT_PASSWORD`).
- Step 2: Use `ANSIBLE_VAULT_PASSWORD_FILE` pointing to a dynamically generated temporary file or script.
- Step 3: Alternatively, write a small shell script (`vault-client.sh`) that fetches the secret dynamically from HashiCorp Vault.
- Step 4: Use multiple Vault IDs (`--vault-id dev@...`, `--vault-id prod@...`) for multi-environment segregation.
- Step 5: Securely erase temporary password files in pipeline cleanup traps.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inject masked CI secret into temporary password file with 600 permissions
export ANSIBLE_VAULT_PASSWORD_FILE=/tmp/.vault_pass && echo "$CI_VAULT_SECRET" > $ANSIBLE_VAULT_PASSWORD_FILE

# Verify that vault password file successfully decrypts production secrets
ansible-vault view group_vars/all/vault.yml --vault-password-file /tmp/.vault_pass

# Execute playbook using specific vault ID credential
ansible-playbook site.yml --vault-id prod@/tmp/.vault_pass

# Securely wipe vault password file immediately after playbook execution
rm -f /tmp/.vault_pass

# Rotate vault encryption password periodically
ansible-vault rekey group_vars/all/vault.yml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In non-interactive CI/CD pipelines, I manage Ansible Vault passwords using `ANSIBLE_VAULT_PASSWORD_FILE`. In GitLab CI or GitHub Actions, the password is stored as a masked protected secret, written to an ephemeral tmpfs file with 600 permissions, and referenced by the runner. Even better, I configure a vault client script (`--vault-id prod@vault_client.py`) that queries HashiCorp Vault via OpenID Connect (OIDC) to retrieve the decryption key dynamically without storing static passwords anywhere in CI."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../06-Infrastructure-as-Code-Terraform-and-OpenTofu-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Configuration Management (Ansible) Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

