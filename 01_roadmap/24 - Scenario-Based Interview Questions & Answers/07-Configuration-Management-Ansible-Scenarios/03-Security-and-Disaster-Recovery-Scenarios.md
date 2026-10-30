# Configuration Management (Ansible) Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Sudo Privilege Escalation Failing for Non-Root Automation User

### 🚨 The Production Scenario
Running an Ansible playbook with `become: yes` fails with `fatal: [node-01]: FAILED! => {"msg": "Missing sudo password"}`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Ansible attempts to elevate privileges using `sudo`. If the remote user configured in Ansible does not have passwordless sudo (`NOPASSWD: ALL`) in `/etc/sudoers.d/`, or if `requiretty` is enabled in sudoers, the command prompts for a password that Ansible cannot provide.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check sudo configuration on target nodes: `sudo cat /etc/sudoers.d/ansible`.
- Step 2: Add passwordless sudo entry: `ansible ALL=(ALL) NOPASSWD: ALL`.
- Step 3: Disable `requiretty` in `/etc/sudoers` (modern Linux distributions disable this by default).
- Step 4: If passwords are required by policy, provide the become password securely via `--ask-become-pass` or HashiCorp Vault.
- Step 5: Verify privilege escalation with `ansible all -m command -a 'whoami' --become`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Grant passwordless sudo privileges to dedicated automation service account
echo 'ansible ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/99-ansible-user

# Set strict required permissions on sudoers drop-in file
sudo chmod 0440 /etc/sudoers.d/99-ansible-user

# Test privilege escalation: output must return 'root'
ansible all -i hosts -m command -a 'whoami' --become

# Prompt interactively for sudo password when passwordless sudo is restricted
ansible-playbook site.yml -K

# Validate sudoers file syntax to prevent host lockout
visudo -c

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Privilege escalation errors occur when the automation user lacks passwordless sudo or requires a TTY. In production environments, our automated provisioning pipeline provisions a dedicated `ansible` user with SSH public key authentication, places a drop-in file in `/etc/sudoers.d/` granting `NOPASSWD: ALL`, and verifies syntax with `visudo -c`. For environments requiring passwords, we inject the become password dynamically from HashiCorp Vault via `ansible_become_password`."

---

## 📌 Scenario 8: Jinja2 Template Rendering Corrupting Production Configuration Files

### 🚨 The Production Scenario
An Ansible role deploying a PostgreSQL `pg_hba.conf` or Nginx configuration renders invalid syntax, crashing the database upon reload.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Jinja2 variable typing mismatches (e.g. booleans vs strings, unquoted IP ranges), missing default filters (`default('localhost')`), or indentation whitespace stripping issues (`{%- ... -%}`) produce malformed configuration outputs.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Use `validate` attribute inside the `template` task to verify syntax BEFORE replacing the production file.
- Step 2: For Nginx: `validate: 'nginx -t -c %s'`.
- Step 3: For Sudoers: `validate: 'visudo -cf %s'`.
- Step 4: Use Jinja2 default filters (`{{ my_var | default('127.0.0.1', true) }}`) to safeguard against undefined variables.
- Step 5: Test template rendering locally using `ansible -m template`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Use validate parameter to test rendered configuration syntax before writing to destination
cat <<EOF > task.yml
- name: Deploy Nginx configuration
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    validate: 'nginx -t -c %s'
EOF

# Inspect exact line-by-line diff rendered by Jinja2 template
ansible-playbook site.yml --diff

# Verify Jinja2 template engine version on control machine
python3 -c 'import jinja2; print(jinja2.__version__)'

# Render template locally on control machine for visual inspection
ansible localhost -m template -a 'src=template.j2 dest=/tmp/test.conf'

# Validate YAML data files feeding variables into templates
yamllint group_vars/

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Never deploy configuration templates without pre-commit validation. Ansible's `template` module features a built-in `validate` parameter (for example, `validate: 'nginx -t -c %s'`). Ansible renders the template to a temporary file `%s`, runs the validation command against it, and ONLY replaces the live production configuration file if the validation exits with code 0. If validation fails, the task aborts and the live service is protected from corrupt config."

---

## 📌 Scenario 9: Molecule Automated Testing for Ansible Roles in CI/CD

### 🚨 The Production Scenario
Developers commit Ansible roles that work on their local Ubuntu machines but fail on enterprise Red Hat / Rocky Linux servers in production.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Differences in package names (`httpd` vs `nginx`), service managers, and default firewall rules break roles across Linux distributions. Without automated integration testing in CI, cross-platform bugs reach production.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Adopt `Molecule` as the standardized testing framework for all Ansible roles.
- Step 2: Configure Molecule scenarios with multiple container drivers (Docker/Podman for Ubuntu and Rocky Linux).
- Step 3: Execute `molecule test` in GitHub Actions / GitLab CI on every pull request.
- Step 4: Molecule automatically executes: linting -> syntax check -> converge (apply) -> idempotence check -> verify (testinfra/goss) -> destroy.
- Step 5: Enforce that all PRs pass `molecule test` before merging.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Initialize a new Molecule testing scenario using Docker container driver
molecule init scenario -d docker

# Execute full automated test cycle: lint, create, converge, idempotency, verify, destroy
molecule test

# Apply the role against ephemeral test containers to inspect live state
molecule converge

# Run Testinfra or Goss assertions to verify ports, files, and services
molecule verify

# Static analysis linter checking for best practices and deprecated syntax
ansible-lint

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "We test Ansible roles exactly like application code: using Molecule and Docker in CI. On every pull request, Molecule spins up clean Docker containers for Ubuntu, Debian, and Rocky Linux, runs `converge` to apply the role, runs it a second time to verify strict idempotency (asserting zero changed tasks), and executes Testinfra assertions verifying that services are listening on port 443. This prevents cross-platform regressions before code hits production."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Configuration Management (Ansible) Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Configuration Management (Ansible) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

