# Configuration Management (Ansible) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Dynamic Rolling Deployments with Ansible and Load Balancers

### 🚨 The Production Scenario
Deploying application updates across 50 backend nodes causes 30 seconds of user-facing connection drops as services restart.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Restarting the backend application on nodes while they remain registered in the Load Balancer (AWS ALB, HAProxy, or F5) routes active client traffic to shutting-down nodes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Set `serial: 10%` to update only 5 nodes at a time.
- Step 2: In `pre_tasks`, deregister the target node from the load balancer target group.
- Step 3: Wait for active connection draining (e.g. 30 seconds).
- Step 4: Apply application updates, restart service, and execute local health check (`curl http://localhost/healthz`).
- Step 5: In `post_tasks`, re-register the healthy node into the load balancer target group before moving to the next batch.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Production rolling deployment playbook with automated load balancer draining
cat <<EOF > rolling_deploy.yml
- hosts: web
  serial: "10%"
  pre_tasks:
    - name: Deregister from ALB
      community.aws.elb_target_group:
        target_group_arn: ...
        targets: [{id: '{{ instance_id }}'}]
        state: absent
    - pause: seconds=20
  tasks:
    - name: Update app
      systemd: name=app state=restarted
  post_tasks:
    - name: Re-register in ALB
      community.aws.elb_target_group:
        targets: [{id: '{{ instance_id }}'}]
        state: present
EOF

# Execute zero-downtime rolling update across target group
ansible-playbook -i hosts rolling_deploy.yml

# Verify target health state transitions in AWS ALB
aws elbv2 describe-target-health --target-group-arn <ARN>

# Verify local health check passes prior to re-enabling traffic
curl -I http://app.internal/health

# Confirm application service is running cleanly
systemctl status app

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Zero-downtime deployments with Ansible require orchestrating load balancer membership alongside updates. I configure `serial: 10%`, execute a `pre_task` using `elb_target_group` to deregister the batch from the load balancer, wait 20 seconds for connection draining, apply the patch, verify local health endpoints via `uri` module, and execute a `post_task` to re-register the node into the target group. Only when the targets report healthy does Ansible proceed to the next batch."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Ansible Playbook Execution Extremely Slow Across 1,000 Nodes** | `cat <<EOF >> ansible.cfg
[defaults]
forks = 50
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts
[ssh_connection]
pipelining = True
ssh_args = -o ControlMaster=auto -o ControlPersist=60m
EOF` | By default, Ansible runs sequentially in small batches (`forks = 5`), establishe... |
| **Scenario 2: Playbook Failing Mid-Run Leaving Production Nodes in an Inconsistent State** | `cat <<EOF > deploy.yml
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
EOF` | Standard Ansible executes tasks top-to-bottom across hosts in a single batch. If... |
| **Scenario 3: Ansible Vault Decryption Failure in Automated CI/CD Pipelines** | `export ANSIBLE_VAULT_PASSWORD_FILE=/tmp/.vault_pass && echo "$CI_VAULT_SECRET" > $ANSIBLE_VAULT_PASSWORD_FILE` | Sensitive files or variables are encrypted with Ansible Vault. In automated CI/C... |
| **Scenario 4: Idempotency Breakdown: Task Re-Executing Every Run with 'changed: true'** | `cat <<EOF > task.yml
- name: Run command idempotently
  command: /opt/app/setup.sh
  args:
    creates: /etc/app/initialized.lock
EOF` | Using raw `command` or `shell` modules instead of declarative Ansible modules (l... |
| **Scenario 5: Dynamic Inventory Failure on Autoscaled Cloud Instances** | `ansible-inventory -i aws_ec2.yml --graph` | Using a static `hosts` file or using dynamic inventory plugins (`amazon.aws.aws_... |
| **Scenario 6: Ansible Handler Fails to Trigger because Preceding Task Skips or Fails** | `cat <<EOF >> deploy.yml
  tasks:
    - name: Update nginx conf
      template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
      notify: Restart Nginx
    - meta: flush_handlers
EOF` | By default, Ansible handlers are queued and execute at the very end of the play.... |
| **Scenario 7: Sudo Privilege Escalation Failing for Non-Root Automation User** | `echo 'ansible ALL=(ALL) NOPASSWD: ALL' \| sudo tee /etc/sudoers.d/99-ansible-user` | Ansible attempts to elevate privileges using `sudo`. If the remote user configur... |
| **Scenario 8: Jinja2 Template Rendering Corrupting Production Configuration Files** | `cat <<EOF > task.yml
- name: Deploy Nginx configuration
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    validate: 'nginx -t -c %s'
EOF` | Jinja2 variable typing mismatches (e.g. booleans vs strings, unquoted IP ranges)... |
| **Scenario 9: Molecule Automated Testing for Ansible Roles in CI/CD** | `molecule init scenario -d docker` | Differences in package names (`httpd` vs `nginx`), service managers, and default... |
| **Scenario 10: Dynamic Rolling Deployments with Ansible and Load Balancers** | `cat <<EOF > rolling_deploy.yml
- hosts: web
  serial: "10%"
  pre_tasks:
    - name: Deregister from ALB
      community.aws.elb_target_group:
        target_group_arn: ...
        targets: [{id: '{{ instance_id }}'}]
        state: absent
    - pause: seconds=20
  tasks:
    - name: Update app
      systemd: name=app state=restarted
  post_tasks:
    - name: Re-register in ALB
      community.aws.elb_target_group:
        targets: [{id: '{{ instance_id }}'}]
        state: present
EOF` | Restarting the backend application on nodes while they remain registered in the ... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Configuration Management (Ansible) Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [AWS Cloud Infrastructure & Solutions Scenarios: Production Incidents & Triage Scenarios →](../08-AWS-Cloud-Infrastructure-and-Solutions-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

