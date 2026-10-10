# 01 - Ansible Role Architecture & Standard Directory Layout

As automation codebases scale from a dozen standalone tasks to thousands of configurations spanning web servers, databases, security baselines, and monitoring agents, storing all logic in single-file playbooks leads to unmaintainable monolithic files. 

Ansible **Roles** solve this problem by introducing a strictly standardized directory convention that decomposes automation into modular, reusable, testable, and shareable software components.

---

## 🏗️ 1. The Standard Role Directory Architecture

An Ansible role is defined entirely by its directory structure. When Ansible encounters a role, it automatically searches predefined subdirectories for `main.yml` files, loading tasks, handlers, variables, defaults, templates, and metadata without requiring manual path imports.

```text
roles/webserver/
├── defaults/
│   └── main.yml         <── Lowest priority default variables (intended to be overridden)
├── vars/
│   └── main.yml         <── High priority immutable role variables (internal constants)
├── tasks/
│   ├── main.yml         <── Primary entry point for tasks executed by this role
│   ├── install.yml      <── Modular subtask file
│   └── configure.yml    <── Modular subtask file
├── handlers/
│   └── main.yml         <── Handlers notified by tasks in this role (service reloads)
├── templates/
│   └── nginx.conf.j2    <── Jinja2 templates processed by the template module
├── files/
│   ├── index.html       <── Static assets copied directly without template rendering
│   └── ssl_dhparam.pem  <── Cryptographic parameters or binary assets
├── meta/
│   └── main.yml         <── Role metadata, supported OS platforms, and dependencies
├── tests/
│   ├── inventory        <── Test inventory for local testing
│   └── test.yml         <── Test harness playbook (used by Molecule / CI)
├── library/             <── (Optional) Custom Python modules shipped inside this role
└── filter_plugins/      <── (Optional) Custom Jinja2 filters shipped inside this role
```

---

## 🔬 2. Deep Dive: Component Responsibilities

| Directory | Primary File | Execution Semantics & Architectural Rules |
| :--- | :--- | :--- |
| `defaults/` | `main.yml` | **Lowest Precedence (Level 2).** Stores sensible defaults (e.g., `http_port: 80`). Consumers of the role can easily override these in `group_vars`, `host_vars`, or playbook `vars`. |
| `vars/` | `main.yml` | **High Precedence (Level 16).** Stores internal constants and distribution-specific paths that role consumers should rarely alter directly (e.g., package names, internal system paths). |
| `tasks/` | `main.yml` | **The Core Execution Flow.** Executes when the role is invoked. Can dynamically import or include sub-files (`install.yml`, `service.yml`) using `ansible.builtin.include_tasks`. |
| `handlers/` | `main.yml` | **Event-Driven Reactions.** Contains service restart or reload actions. Handlers declared here can be notified by tasks within the role, as well as by outside plays. |
| `templates/` | `*.j2` | **Dynamic Configuration Files.** Processed on the control node by Jinja2 before transfer. Files placed here do **not** require prefixing the path with `templates/` in the `template` module. |
| `files/` | `*` | **Static Unaltered Assets.** Shipped directly to target nodes via `copy` or `script`. Files placed here do **not** require prefixing the path with `files/` in the `copy` module. |
| `meta/` | `main.yml` | **Role Dependencies & Metadata.** Specifies upstream roles that must execute before this role runs, alongside supported OS distributions and Galaxy licensing info. |
| `tests/` | `test.yml` | **Automated Testing Harness.** Contains minimal playbook to validate the role locally inside Docker containers or virtual machines via Molecule. |

---

## 🔍 3. Automatic Path Resolution Mechanics

One of the greatest developer-experience advantages of roles is **relative path resolution**. Within any task inside a role, Ansible automatically knows where to find templates, files, and handlers without specifying absolute or relative directory paths:

```yaml
# Inside roles/webserver/tasks/main.yml:

# 1. Ansible automatically searches roles/webserver/files/ for 'index.html'
- name: Deploy static home page
  ansible.builtin.copy:
    src: index.html       # NO NEED for 'files/index.html' or full path
    dest: /var/www/html/index.html

# 2. Ansible automatically searches roles/webserver/templates/ for 'nginx.conf.j2'
- name: Deploy dynamic configuration template
  ansible.builtin.template:
    src: nginx.conf.j2    # NO NEED for 'templates/nginx.conf.j2'
    dest: /etc/nginx/nginx.conf
  notify: Restart Nginx   # Automatically searches roles/webserver/handlers/main.yml
```

---

## 🧭 4. Role Discovery & Search Path Resolution

When a playbook references a role named `webserver`, Ansible searches for the role directory in the following order:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                             ROLE SEARCH PATH RESOLUTION                                │
│                                                                                        │
│   1. Playbook Directory: <playbook_dir>/roles/webserver/                               │
│                                │                                                       │
│                                ▼ (if not found)                                        │
│   2. Configured roles_path in ansible.cfg: roles_path = ./roles:~/.ansible/roles       │
│                                │                                                       │
│                                ▼ (if not found)                                        │
│   3. User Home Directory: ~/.ansible/roles/webserver/                                  │
│                                │                                                       │
│                                ▼ (if not found)                                        │
│   4. System Global Path: /etc/ansible/roles/webserver/                                 │
│                                │                                                       │
│                                ▼ (if not found)                                        │
│   5. Installed Collections: ~/.ansible/collections/ansible_collections/...             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

In `ansible.cfg`:
```ini
[defaults]
roles_path = ./roles:~/.ansible/roles:/opt/shared/ansible/roles
```

---

## ⚡ 5. Execution Order: Roles in the Playbook Lifecycle

When a play defines `pre_tasks`, `roles`, `tasks`, and `post_tasks`, Ansible executes them in a strict chronological sequence:

```text
1. pre_tasks (executed first)
2. pre_tasks handlers (flushed if triggered)
3. roles (executed in the order listed)
4. tasks (regular playbook tasks)
5. post_tasks (executed after all roles and tasks complete)
6. post_tasks handlers (flushed at end of play)
```

### Production Playbook Consuming Roles

```yaml
---
- name: Provision Production Application Cluster
  hosts: app_servers
  become: yes

  vars:
    # Overriding role defaults at the play level
    webserver_http_port: 8080
    security_firewall_allowed_ports:
      - 22
      - 80
      - 8080

  pre_tasks:
    - name: Verify network connectivity to artifact repository
      ansible.builtin.uri:
        url: https://artifacts.internal/health
        status_code: 200

  # Roles execute in strict sequential order
  roles:
    # Role 1: Security baseline
    - role: common_security
      tags: [security, baseline]

    # Role 2: Firewall configuration
    - role: firewall_rules
      tags: [firewall]

    # Role 3: Webserver with custom parameter overrides
    - role: webserver
      vars:
        webserver_worker_processes: 4
      tags: [webserver]

  post_tasks:
    - name: Register node with central monitoring
      ansible.builtin.command:
        cmd: /opt/monitoring/register_node.sh
      changed_when: false
```

---

## 🛠️ 6. Initializing a Role via CLI

To scaffold a standard role adhering to all Ansible best practices, execute:

```bash
ansible-galaxy role init roles/webserver
```

This generates all eight core directories with pre-populated `main.yml` files, ready for production development.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Variables-Facts-and-Jinja2-Templates)](../05-Variables-Facts-and-Jinja2-Templates/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Creating Roles with ansible galaxy role init →](./02-Creating-Roles-with-ansible-galaxy-role-init.md) |
