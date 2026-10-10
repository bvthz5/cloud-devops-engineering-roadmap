# 01 - Ansible Playbook Structure, Plays & YAML Syntax Architecture

Ansible Playbooks are the core declarative blueprints of configuration management and deployment automation. Unlike ad-hoc CLI commands that execute single tasks imperatively, playbooks declare a structured, version-controlled policy of **desired end state**. A playbook is written in YAML and maps groups of managed inventory hosts to ordered series of automated tasks.

---

## 1. Playbook Anatomy: Plays, Tasks, and Directives

A playbook file (`.yml`) contains a list of one or more **Plays**. Each play establishes a context (which hosts to target, what user credentials to use) and executes an ordered pipeline of **Tasks**:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          ANATOMY OF AN ANSIBLE PLAYBOOK                         │
│                                                                                 │
│   site.yml                                                                      │
│   ├── PLAY 1: "Apply Security Baseline" (hosts: all)                            │
│   │   ├── Task 1: Update package cache (ansible.builtin.apt)                    │
│   │   ├── Task 2: Install fail2ban & NTP (ansible.builtin.package)              │
│   │   └── Task 3: Deploy hardened sshd_config (ansible.builtin.template)        │
│   │                                                                             │
│   ├── PLAY 2: "Configure Database Primary" (hosts: db_primary)                  │
│   │   ├── Task 1: Install PostgreSQL 16 (ansible.builtin.package)               │
│   │   ├── Task 2: Configure postgresql.conf (ansible.builtin.template)          │
│   │   └── Handler: Restart PostgreSQL service                                  │
│   │                                                                             │
│   └── PLAY 3: "Deploy API Application Tier" (hosts: webservers)                 │
│       ├── Task 1: Clone Git repository (ansible.builtin.git)                    │
│       ├── Task 2: Build application container (community.docker.docker_image)  │
│       └── Task 3: Start systemd service unit (ansible.builtin.systemd)          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Key Components of a Production Play

Every play combines 6 fundamental directives:

| Play Directive | Purpose & Description | Production Example |
| :--- | :--- | :--- |
| **`name`** | Human-readable label printed during CLI execution and logging. | `name: Configure Production API Cluster` |
| **`hosts`** | Inventory host group, regex, or intersection pattern to target. | `hosts: webservers:&production` |
| **`become`** | Elevates task privileges using `sudo`, `su`, or `pbrun`. | `become: yes` |
| **`vars`** | Play-scoped variables accessible across all tasks in this play. | `vars: { http_port: 443 }` |
| **`tasks`** | Ordered list of module invocations executed sequentially. | `tasks: [ ... ]` |
| **`handlers`** | Event-driven tasks triggered only when tasks register changes. | `handlers: [ ... ]` |

---

## 3. Production Multi-Play Playbook Implementation

```yaml
---
# ==============================================================================
# PLAY 1: Infrastructure Security Baseline (Runs across all nodes)
# ==============================================================================
- name: Apply Enterprise Baseline Hardening
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    required_packages:
      - curl
      - ufw
      - fail2ban
      - unattended-upgrades
    admin_group: devops_admins

  tasks:
    - name: Ensure baseline administration group exists
      ansible.builtin.group:
        name: "{{ admin_group }}"
        state: present
        gid: 1500

    - name: Install foundational security packages
      ansible.builtin.package:
        name: "{{ required_packages }}"
        state: present

    - name: Deploy hardened SSH daemon configuration
      ansible.builtin.template:
        src: templates/sshd_config.j2
        dest: /etc/ssh/sshd_config
        owner: root
        group: root
        mode: '0600'
        validate: '/usr/sbin/sshd -t -f %s'
      notify: Restart SSH Daemon

  handlers:
    - name: Restart SSH Daemon
      ansible.builtin.systemd:
        name: sshd
        state: restarted

# ==============================================================================
# PLAY 2: Web Server Application Setup (Runs only on web tier)
# ==============================================================================
- name: Deploy Web Application Tier
  hosts: webservers
  become: yes
  serial: "50%" # Rolling deployment: updates 50% of the fleet at a time

  vars:
    app_port: 8080
    app_root: /var/www/my-app

  tasks:
    - name: Ensure application directory exists
      ansible.builtin.file:
        path: "{{ app_root }}"
        state: directory
        owner: www-data
        group: www-data
        mode: '0755'

    - name: Ensure Nginx is running and enabled on boot
      ansible.builtin.systemd:
        name: nginx
        state: started
        enabled: yes
```

---

## 4. Fully Qualified Collection Names (FQCN) Standard

In modern Ansible (2.10+ and Ansible Core), modules and plugins are organized into namespaced **Collections**. 

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       FQCN (NAMESPACED) VS. LEGACY SYNTAX                       │
│                                                                                 │
│   ❌ DEPRECATED LEGACY SYNTAX                   ✅ MODERN FQCN STANDARD         │
│   apt:                                          ansible.builtin.apt:            │
│     name: nginx                                   name: nginx                   │
│     state: present                                state: present                │
│                                                                                 │
│   docker_container:                             community.docker.docker_container:
│     name: api_web                                 name: api_web                 │
│                                                                                 │
│   • Ambiguous origin                            • 100% Deterministic origin     │
│   • Namespace collision risk                    • Required by enterprise linting│
└─────────────────────────────────────────────────────────────────────────────────┘
```

The FQCN structure follows: `<NAMESPACE>.<COLLECTION>.<MODULE>`
- Core built-ins: `ansible.builtin.file`, `ansible.builtin.template`, `ansible.builtin.service`.
- Community collections: `community.general.slack`, `community.docker.docker_image`.
- Cloud collections: `amazon.aws.ec2_instance`, `azure.azcollection.azure_rm_virtualmachine`.

---

## 5. Critical YAML Syntax Gotchas in Ansible

1. **The Leading Jinja2 Quoting Trap:**
   In YAML, a line starting with `{` denotes a JSON-style inline mapping. If an argument starts with a Jinja2 variable expression `{{ ... }}`, **it must be enclosed in quotes**:
   ```yaml
   # ❌ SYNTAX ERROR: YAML parser thinks this is a dictionary!
   dest: {{ config_directory }}/app.conf

   # ✅ VALID: Quoted string
   dest: "{{ config_directory }}/app.conf"
   ```
2. **Boolean Parsing Ambiguities:**
   YAML 1.1 parses unquoted `yes`, `no`, `on`, `off`, `true`, `false` as booleans. In modern playbooks, prefer explicit lowercase `true` and `false` or enclose country codes (e.g., country code Norway: `"NO"`) in quotes!
3. **Multiline Strings: Literal (`|`) vs. Folded (`>`):**
   - Literal block scalar (`|`): Preserves newlines (ideal for config files and shell scripts).
   - Folded block scalar (`>`): Replaces newlines with spaces (ideal for long shell command strings).

---

## 6. Pre-Flight Validation CLI Commands

Never run an untested playbook directly against production. Use built-in pre-flight validation tools:

```bash
# 1. Syntax Verification: Parses YAML structure without executing any tasks
ansible-playbook -i hosts.ini site.yml --syntax-check

# 2. List Tasks: Enumerate every task that will execute per host
ansible-playbook -i hosts.ini site.yml --list-tasks

# 3. List Target Hosts: Verify exactly which hosts will be touched
ansible-playbook -i hosts.ini site.yml --list-hosts

# 4. Enterprise Static Analysis via ansible-lint
ansible-lint site.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Ad-Hoc-Commands-and-Core-Modules)](../03-Ad-Hoc-Commands-and-Core-Modules/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Idempotency Principles and Changed Status →](./02-Idempotency-Principles-and-Changed-Status.md) |

