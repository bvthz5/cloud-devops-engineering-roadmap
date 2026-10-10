# 03 - Molecule Framework Architecture, Lifecycle, & Drivers

Writing Ansible roles without automated testing inevitably results in "testing in production": a role that works flawlessly on Ubuntu 20.04 silently breaks when executed against Rocky Linux 9 due to package name differences, firewall configurations, or systemd quirks.

**Molecule** is the official testing framework designed by Red Hat specifically for Ansible roles and collections. It automates testing roles across multiple operating system distributions by provisioning clean, ephemeral test containers or virtual machines, applying the role, auditing idempotency, verifying desired state assertions, and destroying the test environment upon completion.

---

## 🏗️ 1. Molecule Architecture & Directory Structure

When Molecule is initialized within an Ansible role, it creates a `molecule/` directory containing one or more test scenarios (with `default` being the primary scenario):

```text
roles/webserver/
├── tasks/
│   └── main.yml
├── templates/
│   └── nginx.conf.j2
└── molecule/
    └── default/
        ├── molecule.yml   <── Primary scenario configuration (driver, platforms, verifier)
        ├── converge.yml   <── Test playbook invoking the role under test
        ├── prepare.yml    <── (Optional) Pre-test setup (e.g., installing epel-release)
        ├── verify.yml     <── Automated assertion tests checking post-deployment state
        └── cleanup.yml    <── (Optional) Post-test teardown actions
```

---

## 🔄 2. The Complete Molecule Test Lifecycle

When an engineer or CI runner executes `molecule test`, Molecule executes a strictly ordered 12-stage automated testing pipeline:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                MOLECULE TEST PIPELINE                                  │
│                                                                                        │
│   1. dependency  ──► Pulls Galaxy roles/collections required by the role               │
│   2. lint        ──► Runs yamllint, ansible-lint, and flake8                           │
│   3. cleanup     ──► Cleans up dangling resources from prior failed runs               │
│   4. destroy     ──► Ensures no previous test containers exist                         │
│   5. syntax      ──► Validates playbook YAML syntax with ansible-playbook              │
│   6. create      ──► Provisions ephemeral instances via driver (Docker/Podman/VMs)     │
│   7. prepare     ──► Executes prepare.yml to install OS prerequisites                  │
│   8. converge    ──► Executes converge.yml to apply the role to test instances         │
│   9. idempotence ──► Re-runs converge.yml; FAILS if any task reports 'changed: true'   │
│  10. side_effect ──► (Optional) Tests state transitions / HA failover                  │
│  11. verify      ──► Executes verify.yml assertions (tests ports, files, responses)    │
│  12. destroy     ──► Tears down and removes all ephemeral test instances               │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Essential Developer CLI Workflows

While `molecule test` executes the complete pipeline, developers writing roles use granular commands for rapid feedback:

```bash
# 1. Spin up the container and apply the role (keeps container running)
molecule converge

# 2. Drop into a bash shell inside the test container for manual inspection
molecule login --host instance-ubuntu2204

# 3. Re-run verification tests without rebuilding containers
molecule verify

# 4. Check whether your tasks are truly idempotent
molecule idempotence

# 5. Destroy the test container when finished
molecule destroy
```

---

## 🔌 3. Molecule Drivers: Container vs. VM Matrix

Molecule decouples test logic from the underlying infrastructure through pluggable **Drivers**:

| Driver | Plugin Package | Infrastructure Type | Best Use Case |
| :--- | :--- | :--- | :--- |
| **Docker** | `molecule-plugins[docker]` | Ephemeral Docker containers | **Fastest, standard for CI/CD**. Tests packages, files, user permissions, and ports. |
| **Podman** | `molecule-plugins[podman]` | Rootless, daemonless containers | Modern Red Hat / Enterprise Linux environments; rootless CI pipelines. |
| **Vagrant** | `molecule-plugins[vagrant]` | VirtualBox / libvirt VMs | Heavy roles requiring true systemd init, kernel modules, or custom network interfaces. |
| **Delegated** | Built-in | Pre-existing VMs or cloud instances | Testing against real AWS EC2 instances, OpenStack clusters, or physical hardware. |

---

## ⚙️ 4. Production Scenario Configuration: `molecule/default/molecule.yml`

Here is an enterprise-grade `molecule.yml` configured to test a role across both Debian (Ubuntu 22.04) and Enterprise Linux (Rocky Linux 9) in parallel:

```yaml
---
dependency:
  name: galaxy

driver:
  name: docker

platforms:
  # Platform 1: Ubuntu 22.04 LTS
  - name: instance-ubuntu2204
    image: geerlingguy/docker-ubuntu2204-ansible:latest
    pre_build_image: true
    privileged: true
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    command: "/lib/systemd/systemd"

  # Platform 2: Rocky Linux 9 (RHEL Compatible)
  - name: instance-rockylinux9
    image: geerlingguy/docker-rockylinux9-ansible:latest
    pre_build_image: true
    privileged: true
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    command: "/usr/sbin/init"

provisioner:
  name: ansible
  options:
    vvv: false
  inventory:
    links:
      hosts: hosts.ini

verifier:
  name: ansible
```

> **Note on Systemd in Containers:** Roles managing daemons via `ansible.builtin.systemd` require systemd to run as PID 1 inside the container. Mounting `/sys/fs/cgroup` and using pre-baked systemd container images (such as `geerlingguy/docker-*-ansible`) enables full systemd service lifecycle testing inside Docker.

---

## 📄 5. The Test Harness Playbooks

### `molecule/default/converge.yml`
This playbook applies the role to the test instances:

```yaml
---
- name: Converge Test Scenario
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    webserver_http_port: 8080
    webserver_enable_monitoring: true

  roles:
    - role: webserver
```

### `molecule/default/verify.yml`
This playbook runs verification assertions after convergence:

```yaml
---
- name: Verify Webserver Configuration
  hosts: all
  become: yes
  gather_facts: no

  tasks:
    # 1. Assert Nginx package is installed
    - name: Check Nginx package status
      ansible.builtin.package:
        name: nginx
        state: present
      check_mode: yes
      register: pkg_status
      failed_when: pkg_status.changed

    # 2. Assert Nginx service is running and enabled
    - name: Check Nginx systemd service
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes
      check_mode: yes
      register: srv_status
      failed_when: srv_status.changed

    # 3. Assert HTTP listener responds on port 8080
    - name: Query webserver HTTP endpoint
      ansible.builtin.uri:
        url: "http://127.0.0.1:8080"
        status_code: 200
        return_content: yes
      register: http_res
      failed_when: "'Welcome' not in http_res.content"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Static Analysis with ansible lint and yamllint](./02-Static-Analysis-with-ansible-lint-and-yamllint.md) | [Index](../../../README.md) | [04 - Writing Molecule Test Scenarios and Verifiers →](./04-Writing-Molecule-Test-Scenarios-and-Verifiers.md) |
