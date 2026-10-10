# 01 - Static Inventories: INI vs. YAML Formats & Host Pattern Algebra

In Ansible, the **Inventory** is the definitive database defining the hosts, IP addresses, groups, and behavioral connection parameters that Ansible orchestrates. While dynamic inventories query cloud APIs at runtime, static inventories provide deterministic, version-controlled definitions for on-premises bare-metal servers, staging clusters, and fixed cloud instances.

---

## 1. INI vs. YAML: Deep Architectural Comparison

Ansible supports two standard static formats: **INI** (legacy, concise) and **YAML** (modern, structured):

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       STATIC INVENTORY FORMAT TRADE-OFFS                        │
│                                                                                 │
│   INI FORMAT (Concise / Legacy)                 YAML FORMAT (Structured / Modern)│
│   ┌───────────────────────────────────┐         ┌─────────────────────────────┐ │
│   │ [web]                             │         │ all:                        │ │
│   │ web1 ansible_host=10.0.1.5        │         │   children:                 │ │
│   │ web2 ansible_host=10.0.1.6        │         │     web:                    │ │
│   │                                   │         │       hosts:                │ │
│   │ [web:vars]                        │         │         web1:               │ │
│   │ http_port=80                      │         │           ansible_host: ... │ │
│   │ ssl_enabled=true                  │         │       vars:                 │ │
│   └───────────────────────────────────┘         │         ssl_enabled: true   │ │
│   • Extremely fast to write                     └─────────────────────────────┘ │
│   • Hard to parse nested dictionaries           • Full support for lists/dicts  │
│   • String-heavy typing                         • Strict data types & validation│
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Production Syntax Implementations

### Production INI Inventory (`inventories/production/hosts.ini`)

```ini
# Individual hosts with connection parameters
bastion.corp.internal ansible_host=203.0.113.10 ansible_user=bastion_admin

# Group: Webservers with host range shortcuts
[webservers]
web[01:04].prod.internal http_port=8080 max_clients=500
web05.prod.internal ansible_host=10.0.1.25

# Group: Database primary and replicas
[dbservers]
db-primary.prod.internal ansible_host=10.0.2.10 db_role=primary
db-replica[01:02].prod.internal db_role=replica

# Nested Children Groups (Metagroups)
[production:children]
webservers
dbservers

# Group Variables applied to all hosts in the 'production' metagroup
[production:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/prod_ed25519
ansible_python_interpreter=/usr/bin/python3
environment_name=production
```

---

### Production YAML Inventory (`inventories/production/hosts.yml`)

The YAML format is the modern enterprise standard because it natively represents structured configuration objects (arrays, dictionaries, booleans):

```yaml
all:
  vars:
    ansible_user: ubuntu
    ansible_ssh_private_key_file: ~/.ssh/prod_ed25519
    ansible_python_interpreter: /usr/bin/python3
    ntp_servers:
      - 0.pool.ntp.org
      - 1.pool.ntp.org
  children:
    edge_network:
      hosts:
        bastion.corp.internal:
          ansible_host: 203.0.113.10
          ansible_port: 2222
    production:
      children:
        webservers:
          vars:
            http_port: 8080
            ssl_certificate: /etc/ssl/certs/prod.crt
          hosts:
            web01.prod.internal:
              ansible_host: 10.0.1.21
            web02.prod.internal:
              ansible_host: 10.0.1.22
        dbservers:
          hosts:
            db01.prod.internal:
              ansible_host: 10.0.2.10
              db_role: primary
            db02.prod.internal:
              ansible_host: 10.0.2.11
              db_role: replica
```

---

## 3. Core Behavioral Host Variables Reference

Ansible reserves special variable names starting with `ansible_` that dictate how the transport layer connects and executes tasks on target machines:

| Behavioral Variable | Type | Purpose & Production Description |
| :--- | :--- | :--- |
| **`ansible_host`** | string | The real IP address or DNS resolvable FQDN of the target server. |
| **`ansible_port`** | integer | Custom SSH or WinRM port (defaults to `22` for SSH, `5986` for HTTPS WinRM). |
| **`ansible_user`** | string | The username used to log in via SSH (e.g., `ubuntu`, `ec2-user`, `ansible`). |
| **`ansible_ssh_private_key_file`** | path | Filepath to the private key used for authentication. |
| **`ansible_connection`** | string | Transport protocol: `ssh`, `winrm`, `local`, `docker`, `podman`, `kubectl`. |
| **`ansible_become`** | boolean | Whether to automatically escalate privileges (`true` / `false`). |
| **`ansible_become_method`** | string | Privilege escalation tool (`sudo`, `su`, `pbrun`, `doas`). |
| **`ansible_become_user`** | string | Target user for privilege escalation (defaults to `root`). |
| **`ansible_python_interpreter`** | path | Explicit path to the target machine's Python binary (e.g., `/usr/bin/python3`). |
| **`ansible_ssh_common_args`** | string | Raw SSH CLI flags, such as ProxyJump bastion directives. |

---

## 4. Host Pattern Algebra in Ansible CLI

When executing ad-hoc commands or playbooks, Ansible provides mathematical pattern matching to slice and filter target hosts dynamically:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          HOST PATTERN ALGEBRA MATRIX                            │
│                                                                                 │
│   PATTERN SYNTAX            MEANING                     BOOLEAN LOGIC           │
│   ───────────────────────── ─────────────────────────── ─────────────────────── │
│   all (or *)                Every host in inventory     Universe                │
│   webservers:dbservers      Union of both groups        Group A OR Group B      │
│   webservers:&production    Intersection of groups      Group A AND Group B     │
│   webservers:!staging       Difference / Exclusion      Group A AND NOT Group B │
│   webservers[0:2]           Subscript range slice       Index 0 to 2            │
│   ~web\d+\.prod\.corp       Regex pattern match         Matches Regex Pattern   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Production CLI Pattern Examples:

```bash
# 1. Target all webservers AND dbservers simultaneously (Union)
ansible 'webservers:dbservers' -m ping

# 2. Target only servers that belong to BOTH 'webservers' and 'production' (Intersection)
ansible 'webservers:&production' -m ping

# 3. Target webservers, EXCLUDING any that are tagged in 'canary' (Exclusion)
ansible 'webservers:!canary' -m command -a "uptime"

# 4. Target the first 3 webservers using slice indexing
ansible 'webservers[0:2]' -m ping

# 5. Target hosts matching a complex regular expression
ansible '~^app-(east|west)-\d+\.internal$' -m ping
```

---

## 5. Inspecting & Validating Inventories via CLI

Never guess what hosts Ansible will target. Always use the built-in **`ansible-inventory`** command to validate parsing:

```bash
# Render the complete inventory hierarchy as an ASCII graph
ansible-inventory -i hosts.yml --graph

# Output:
# @all:
#   |--@edge_network:
#   |  |--bastion.corp.internal
#   |--@production:
#   |  |--@webservers:
#   |  |  |--web01.prod.internal
#   |  |  |--web02.prod.internal
#   |  |--@dbservers:
#   |  |  |--db01.prod.internal
#   |  |  |--db02.prod.internal

# Dump the parsed JSON structure showing all inherited variables for a specific host
ansible-inventory -i hosts.yml --host web01.prod.internal
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Ansible-Architecture-Agentless-Push)](../01-Ansible-Architecture-Agentless-Push/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Host Groups Nested Groups and Group Vars →](./02-Host-Groups-Nested-Groups-and-Group-Vars.md) |

