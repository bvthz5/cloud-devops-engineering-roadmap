# 02 - Ansible Facts & Gathering System Metadata

Ansible Facts are granular, real-time system discoveries automatically collected from managed nodes before tasks execute. By inspecting the physical hardware, operating system distribution, kernel version, network interfaces, IP addresses, mounted filesystems, and virtualization hypervisor of each host, facts enable a single Ansible playbook to configure heterogeneous operating systems dynamically without hardcoding environment specifics.

---

## 🏗️ 1. How Fact Gathering Works: The `setup` Module

Unless explicitly disabled with `gather_facts: false`, Ansible transparently injects an implicit task at the beginning of every play: `TASK [Gathering Facts]`. 

Under the hood, Ansible packages and executes the `ansible.builtin.setup` module on each target node. The module queries system APIs, kernel subsystems (`/proc`, `/sys`), and administrative tools (`ip`, `dmidecode`, `lsblk`, `facter`, `ohai`), serializes the discovered metadata into a structured JSON tree, and injects it into memory under the `ansible_facts` dictionary.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          ANSIBLE FACT GATHERING LIFECYCLE                              │
│                                                                                        │
│   CONTROL NODE                                           MANAGED NODE                  │
│                                                                                        │
│   1. Play starts (hosts: webservers)                                                   │
│   2. Implicit Task: Gathering Facts                                                    │
│   3. Ansiballz packages 'setup' module ────(OpenSSH)───► 4. Python executes 'setup'   │
│                                                             Inspects /proc, /sys       │
│                                                             Queries network interfaces │
│                                                             Reads /etc/os-release      │
│                                                             Executes facts.d scripts   │
│                                                                    │                   │
│   6. Injects into memory under       ◄──(JSON Stream)─── 5. Emits JSON Fact Tree       │
│      ansible_facts dictionary                                                          │
│   7. Tasks evaluate facts:                                                             │
│      when: ansible_facts['os_family'] == "Debian"                                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔬 2. Anatomy of Standard Ansible Facts

Modern Ansible namespaces all gathered facts inside the `ansible_facts` dictionary. (Legacy code used unprefixed variables like `ansible_distribution`, but using `ansible_facts['key']` is the strict modern best practice to prevent variable collisions).

### Essential Fact Reference

| Fact Category | Key Syntax (`ansible_facts[...]`) | Example Real-World Return Value | Production Use Case |
| :--- | :--- | :--- | :--- |
| **OS Distribution** | `['distribution']` | `"Ubuntu"`, `"RedHat"`, `"CentOS"` | Conditional package repository setup |
| **OS Family** | `['os_family']` | `"Debian"`, `"RedHat"`, `"Suse"` | High-level package manager routing (`apt` vs `yum`) |
| **Kernel Version** | `['kernel']` | `"5.15.0-89-generic"` | Verifying kernel patches for security compliance |
| **FQDN / Hostname** | `['fqdn']`, `['hostname']` | `"web01.prod.internal"`, `"web01"` | Dynamic configuration file / DNS templating |
| **Primary IPv4** | `['default_ipv4']['address']` | `"10.0.1.25"` | Binding web server listeners to specific interfaces |
| **Default Gateway** | `['default_ipv4']['gateway']` | `"10.0.1.1"` | Validating network routing and routing table entries |
| **All IPv4s** | `['all_ipv4_addresses']` | `["10.0.1.25", "172.17.0.1"]` | Whitelisting firewall rules across internal subnets |
| **CPU Cores** | `['processor_vcpus']` | `8` | Calculating worker thread pools for Nginx/Gunicorn |
| **Total Memory** | `['memtotal_mb']` | `16384` | Sizing database buffer pools (e.g., 75% of RAM) |
| **Storage Mounts** | `['mounts']` | `[{"mount": "/", "size_available": ...}]`| Alerting if root filesystem disk space is below 10% |
| **Virtualization** | `['virtualization_type']` | `"kvm"`, `"docker"`, `"aws"` | Applying cloud-specific or container-specific tuning |

---

## ⚡ 3. The Performance Overhead & Optimization Strategies

While facts provide powerful system awareness, collecting hundreds of hardware, network, and storage properties takes between **1 to 5 seconds per managed node**. When executing playbooks across fleets of 1,000+ servers, fact gathering becomes a severe performance bottleneck.

### Strategy 1: Disabling Facts When Not Needed

If your playbook only deploys an application binary or executes API tasks that do not depend on system facts, disable fact gathering explicitly:

```yaml
- name: Deploy Pre-Baked Container Image
  hosts: webservers
  gather_facts: false   # Instantly saves 2-5 seconds per host
  tasks:
    - name: Pull Docker image
      community.docker.docker_image:
        name: myapp:v2.1
        source: pull
```

### Strategy 2: Granular Fact Filtering with `gather_subset`

Rather than gathering all facts or none, use `gather_subset` to collect only the minimal facts required:

```yaml
- name: Optimize Network Configuration
  hosts: all
  gather_subset:
    - '!all'       # Exclude the full default fact collection
    - '!min'       # Exclude standard minimal facts
    - 'network'    # Only collect network interface and IP facts
```

Available subsets include: `all`, `min`, `hardware`, `network`, `virtual`, `facter`, and `ohai`. Prefixing with `!` negates the subset.

### Strategy 3: Enterprise Fact Caching (Redis / JSON)

Instead of re-querying nodes on every single playbook run, Ansible can cache facts in a central persistent store like Redis or local JSON files.

In `ansible.cfg`:

```ini
[defaults]
gathering = smart
fact_caching = redis
fact_caching_connection = 127.0.0.1:6379:0
fact_caching_timeout = 86400   # Cache facts for 24 hours
```

With `gathering = smart`, Ansible only gathers facts if the cache has expired or does not contain an entry for the target host, cutting playbook runtimes drastically.

---

## 📁 4. Custom Local Facts (`/etc/ansible/facts.d/*.fact`)

In enterprise environments, systems administrators often need to inject business metadata that the operating system cannot detect automatically, such as server environment (`prod`, `staging`), datacenter rack identifier, business unit owner, or primary database role.

Ansible supports **Custom Local Facts** placed inside `/etc/ansible/facts.d/` on the managed node:

### Format A: Static JSON or INI File (`/etc/ansible/facts.d/tier.fact`)

```json
{
    "environment": "production",
    "datacenter": "us-east-dc2",
    "rack_id": "rack-42-b",
    "backup_window": "03:00-04:00"
}
```

### Format B: Dynamic Executable Script (`/etc/ansible/facts.d/cluster.fact`)

Any executable file (`chmod +x`) returning valid JSON to standard output is executed during fact gathering:

```bash
#!/bin/bash
# Query local daemon or metadata service
PRIMARY_NODE=$(curl -s http://127.0.0.1:8500/v1/leader | tr -d '"')
echo "{\"consul_leader\": \"${PRIMARY_NODE}\"}"
```

### Consuming Local Facts in Playbooks

All local facts are automatically structured under the `ansible_local` namespace:

```yaml
- name: Display Custom Infrastructure Metadata
  hosts: all
  tasks:
    - name: Print custom rack location
      ansible.builtin.debug:
        msg: "Host is in {{ ansible_facts['ansible_local']['tier']['datacenter'] }} on rack {{ ansible_facts['ansible_local']['tier']['rack_id'] }}"
```

---

## 💻 5. Ad-Hoc Fact Exploration via CLI

To inspect what facts a target host returns without authoring a playbook, use the ad-hoc `setup` module:

```bash
# Print all discovered facts for a specific host in JSON format
ansible web1.prod.internal -m setup

# Filter facts using a glob pattern (e.g., only memory facts)
ansible web1.prod.internal -m setup -a "filter=ansible_mem*"

# Filter network interface facts
ansible web1.prod.internal -m setup -a "filter=ansible_default_ipv4"
```

---

## 🎯 6. Complete Production Playbook: Fact-Driven Dynamic Configuration

```yaml
---
- name: Configure Web Server Fleet Dynamically
  hosts: webservers
  gather_facts: yes
  become: yes

  tasks:
    # 1. OS-Family Conditional Package Installation
    - name: Install Apache on Debian family (Ubuntu / Debian)
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: yes
      when: ansible_facts['os_family'] == "Debian"

    - name: Install Apache on RedHat family (RHEL / Rocky / Alma)
      ansible.builtin.dnf:
        name: httpd
        state: present
      when: ansible_facts['os_family'] == "RedHat"

    # 2. Dynamic Hardware-Based Tuning
    - name: Tune worker connections based on CPU cores
      ansible.builtin.template:
        src: templates/httpd-tuning.conf.j2
        dest: /etc/httpd/conf.d/tuning.conf
      vars:
        # Dynamically allocate 256 connections per CPU core
        max_clients: "{{ (ansible_facts['processor_vcpus'] | int) * 256 }}"
      notify: Restart Web Server

    # 3. Network-Specific Service Binding
    - name: Bind application listener to primary private IP
      ansible.builtin.lineinfile:
        path: /etc/app/config.ini
        regexp: '^ListenAddress='
        line: "ListenAddress={{ ansible_facts['default_ipv4']['address'] }}"

  handlers:
    - name: Restart Web Server
      ansible.builtin.service:
        name: "{{ (ansible_facts['os_family'] == 'Debian') | ternary('apache2', 'httpd') }}"
        state: restarted
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ansible Variable Precedence 22 Levels](./01-Ansible-Variable-Precedence-22-Levels.md) | [Index](../../../README.md) | [03 - Registering Variables and Task Output Capture →](./03-Registering-Variables-and-Task-Output-Capture.md) |
