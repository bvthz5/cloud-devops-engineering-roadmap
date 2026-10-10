# 01 - Ansible Variable Precedence: The Complete 22-Level Hierarchy

In Ansible, variables can be defined in dozens of locations: inventory files, host directories, role defaults, role vars, playbooks, registered tasks, system facts, and command-line flags. When the exact same variable name (e.g., `http_port`) is declared in multiple places, Ansible resolves the conflict using a strict **22-level precedence hierarchy**. 

Understanding this hierarchy prevents "variable shadowing" bugs and dictates where variables belong in enterprise role design.

---

## 1. The Variable Precedence Pyramid (Lowest to Highest)

The golden rule of Ansible variable resolution is: **The higher the number, the higher the precedence. Level 22 overrides all other definitions!**

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      THE ANSIBLE VARIABLE PRECEDENCE PYRAMID                    │
│                                                                                 │
│   [LEVEL 22] extra vars (-e in CLI)               ◄── HIGHEST (Overrides ALL!)  │
│   ─────────────────────────────────────────────                                 │
│   [LEVEL 21] task vars (only for the task)                                      │
│   [LEVEL 20] task include_vars                                                  │
│   [LEVEL 19] role vars (in role/vars/main.yml)    ◄── Role Constants (Locked)   │
│   [LEVEL 18] block vars (only for tasks in block)                               │
│   [LEVEL 17] play vars_files                                                    │
│   [LEVEL 16] play vars_prompt                                                   │
│   [LEVEL 15] play vars                                                          │
│   [LEVEL 14] set_fact / registered task vars                                    │
│   [LEVEL 13] host facts / discovered facts                                      │
│   [LEVEL 12] playbook host_vars                                                 │
│   [LEVEL 11] inventory host_vars                                                │
│   [LEVEL 10] inventory file host_vars                                           │
│   [LEVEL 09] playbook group_vars                                                │
│   [LEVEL 08] inventory group_vars                                               │
│   [LEVEL 07] inventory file group_vars                                          │
│   [LEVEL 06] playbook group_vars/all                                            │
│   [LEVEL 05] inventory group_vars/all                                           │
│   [LEVEL 04] inventory file group_vars/all                                      │
│   [LEVEL 03] inventory file or script default vars                             │
│   [LEVEL 02] role params (include_role / import_role)                          │
│   [LEVEL 01] role defaults (role/defaults/main.yml)◄── LOWEST (Easily Overridden)│
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The Complete 22-Level Precedence Table (Official Specification)

| Precedence Level | Variable Scope / Location | Where It Is Defined | Intended Enterprise Use Case |
| :---: | :--- | :--- | :--- |
| **1 (Lowest)** | **role defaults** | `roles/<name>/defaults/main.yml` | Default fallback values intended to be easily overridden by consumers. |
| **2** | **inventory file or script group_vars/all** | Group `all` inside static INI/YAML | Default global settings across an entire inventory. |
| **3** | **inventory group_vars/all** | Directory `group_vars/all.yml` | Shared baseline variables (e.g., standard DNS/NTP servers). |
| **4** | **playbook group_vars/all** | Directory `group_vars/all.yml` relative to playbook | Global playbook-specific fallbacks. |
| **5** | **inventory file or script group_vars/\*** | Target group inside INI/YAML | Settings applied to specific inventory groups. |
| **6** | **inventory group_vars/\*** | Directory `group_vars/<group_name>.yml` | Group-specific variables (e.g., `group_vars/webservers.yml`). |
| **7** | **playbook group_vars/\*** | Directory `group_vars/<group_name>.yml` relative to play | Playbook-specific group overrides. |
| **8** | **inventory file or script host_vars/\*** | Explicit host entry in INI/YAML | Specific host connection overrides in inventory. |
| **9** | **inventory host_vars/\*** | Directory `host_vars/<hostname>.yml` | Host-specific parameters (e.g., node-specific IP or MAC address). |
| **10** | **playbook host_vars/\*** | Directory `host_vars/<hostname>.yml` relative to play | Host-specific overrides within a playbook repo. |
| **11** | **host facts / cached facts** | Gathered via `setup` module | Dynamic hardware/kernel metrics discovered from target hosts. |
| **12** | **play vars** | Play directive `vars:` in playbook | Variables defined directly on a play. |
| **13** | **play vars_prompt** | Interactive prompts | User inputs gathered via CLI prompts during playbook startup. |
| **14** | **play vars_files** | Play directive `vars_files:` | Variables imported from external YAML files. |
| **15** | **role vars** | `roles/<name>/vars/main.yml` | **Role-internal constants.** Not meant to be overridden by users! |
| **16** | **block vars** | Block directive `vars:` | Scoped only to tasks nested within a specific `block:`. |
| **17** | **task vars** | Task directive `vars:` | Scoped strictly to that single task. |
| **18** | **include_vars** | Dynamic task `include_vars` | Variables loaded dynamically at runtime based on OS facts. |
| **19** | **set_fact / registered vars** | `set_fact` or `register:` | Dynamic values generated and captured during task execution. |
| **20** | **role params** | Parameters passed into `roles:` | Arguments passed when calling a role: `roles: [{ role: web, port: 80 }]`. |
| **21** | **include_role / import_role params** | Parameters passed into task | Arguments passed via `ansible.builtin.include_role: { vars: ... }`. |
| **22 (Highest)** | **extra vars** | CLI flag `-e` or `--extra-vars` | **Unconditional override.** Overrides every other definition in Ansible! |

---

## 3. The Core Architectural Debate: `defaults/main.yml` vs. `vars/main.yml`

One of the most frequent junior DevOps mistakes is placing user-configurable variables inside a role's `vars/main.yml` instead of `defaults/main.yml`.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    ROLE DEFAULTS VS. ROLE VARS ARCHITECTURE                     │
│                                                                                 │
│   ROLES/DEFAULTS/MAIN.YML (Precedence Level 1)                                  │
│   • Purpose: User-facing configuration defaults                                 │
│   • Behavior: Easily overridden by inventory, group_vars, or playbook vars      │
│   • Example: nginx_port: 80, nginx_worker_processes: "auto"                    │
│                                                                                 │
│   ROLES/VARS/MAIN.YML (Precedence Level 15)                                     │
│   • Purpose: Internal role implementation constants                             │
│   • Behavior: VERY DIFFICULT to override (Outranks play vars and group_vars!)   │
│   • Example: nginx_debian_package: "nginx-full", nginx_redhat_pkg: "nginx"      │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The SRE Golden Rule:
- If a user of your role might ever need to change the value (e.g., port numbers, memory limits, URLs), put it in **`defaults/main.yml`**.
- Only put values in **`vars/main.yml`** if they are internal OS-specific mappings that must never be altered by the playbook author.

---

## 4. Production Variable Precedence in Action

Consider this example where `app_port` is declared across 4 different levels:

```yaml
# 1. Inside role: roles/api/defaults/main.yml (Level 1)
app_port: 3000

# 2. Inside group_vars: inventories/group_vars/webservers.yml (Level 6)
app_port: 8080

# 3. Inside playbook: site.yml (Level 12)
- name: Deploy API
  hosts: webservers
  vars:
    app_port: 9000
```

### What value wins?
- If run normally (`ansible-playbook -i hosts site.yml`):
  - **Level 12 (Play vars: `9000`)** wins over `8080` (group_vars) and `3000` (defaults).
- If run with extra vars (`ansible-playbook -i hosts site.yml -e "app_port=4444"`):
  - **Level 22 (Extra vars: `4444`)** wins over everything!

---

## 5. Enterprise Best Practices Checklist

1. **Avoid `vars/main.yml` for Configurable Settings:** Always use `defaults/main.yml` so consumer teams can override settings via `group_vars`.
2. **Never Rely on Precedence Level Battles:** If your playbooks require complex mental gymnastics to determine which variable wins, refactor your variable naming (e.g., prefix role variables with `rolename_`).
3. **Use `-e` Sparingly:** Extra variables (`-e`) bypass all code governance and cannot be overridden. Restrict `-e` usage to dynamic CI/CD build identifiers or test overrides.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Ansible-Playbooks-Tasks-and-Handlers)](../04-Ansible-Playbooks-Tasks-and-Handlers/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Ansible Facts and Gathering System Metadata →](./02-Ansible-Facts-and-Gathering-System-Metadata.md) |

