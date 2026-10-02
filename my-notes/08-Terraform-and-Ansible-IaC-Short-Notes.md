# 🏗️ Terraform & Ansible (IaC) — Complete Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, advantages, real-life analogies, and command breakdown. Covers every Terraform concept, state management, CLI commands, Ansible architecture, playbooks, roles, and vault in 1-line format.

---

## 🧠 1. Infrastructure as Code (IaC) Fundamentals

| Concept | Simple 1-Line Definition (Kids Mind) | Real-Life Analogy | Main Advantage |
| :--- | :--- | :--- | :--- |
| **Infrastructure as Code (IaC)**| Writing human-readable code files to automatically create, modify, and delete servers and networks. | A 3D blueprint fed into a 3D printer that manufactures a building automatically. | Rebuild an entire cloud data center in 15 minutes with zero human manual errors. |
| **Declarative (Terraform)**| You declare **WHAT** end result you want; the tool figures out how to make it happen. | Ordering dinner at a restaurant ("I want a medium burger with fries"). | You don't write complex loops or check if a server already exists; tool handles it. |
| **Imperative (Bash / Python)**| You write step-by-step instructions detailing **HOW** to build every piece. | Writing a 20-page cookbook detailing every knife cut and stove temperature. | Hard to maintain; scripts break if a server is already partially created. |
| **Provisioning vs Config Mgmt**| **Provisioning (Terraform)** creates raw hardware (VPC, VMs, S3); **Configuration (Ansible)** installs software inside those VMs. | Terraform builds the empty house; Ansible paints the walls and installs appliances. | Use both together for a 100% automated enterprise infrastructure stack. |
| **Idempotency** | The golden rule: Running the exact same code 10 times produces the exact same result without duplicate changes. | Pressing the "Light ON" switch when the light is already on; nothing changes. | Safe to re-run automation scripts repeatedly without breaking production. |
| **Immutable Infrastructure** | Never patch or edit running production servers; build a new VM image and replace the old one! | Throwing away a paper cup instead of washing and repairing it. | Zero configuration drift; every server is 100% identical and tested. |

---

## 🌍 2. Terraform Architecture & Core Concepts

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        TERRAFORM WORKFLOW FLOW                         │
│                                                                        │
│   ┌────────────────────┐          ┌────────────────────┐               │
│   │   main.tf CODE     │          │  REMOTE PROVIDER   │               │
│   │ (Desired State)    │          │  (AWS / Azure API) │               │
│   └─────────┬──────────┘          └─────────▲──────────┘               │
│             │                               │                          │
│             ▼                               │                          │
│   ┌────────────────────┐                    │                          │
│   │  terraform plan    │────────────────────┤                          │
│   │ (Calculates Diff)  │                    │                          │
│   └─────────┬──────────┘                    │                          │
│             │                               │                          │
│             ▼                               │                          │
│   ┌────────────────────┐          ┌─────────┴──────────┐               │
│   │  terraform apply   │─────────►│ terraform.tfstate  │               │
│   │ (Provisions Cloud) │          │ (Real-World State) │               │
│   └────────────────────┘          └────────────────────┘               │
└────────────────────────────────────────────────────────────────────────┘
```

### Core Concepts (1-Line Definitions):
- **HCL (HashiCorp Configuration Language):** The human-friendly, declarative coding syntax used to write Terraform files (`.tf`).
- **Provider:** A software plugin connecting Terraform to a specific cloud or service API (`provider "aws"`, `provider "azure"`, `provider "kubernetes"`).
- **Resource:** The fundamental block declaring a real cloud infrastructure object to create (`resource "aws_instance" "web"`).
- **Data Source:** A query block that fetches information about existing resources already created in the cloud without managing them (`data "aws_ami" "ubuntu"`).
- **Variable (`variable`):** Input parameters that make your Terraform code customizable and reusable across dev, staging, and prod.
- **Output (`output`):** Return values printed to terminal or passed to other tools (e.g., printing the newly created EC2 public IP address).
- **Locals (`locals`):** Internal named expressions or shortcuts used within a module to avoid repeating complex formulas.
- **State File (`terraform.tfstate`):** The digital database map recording the exact IDs and configurations of every real-world cloud resource managed by Terraform.
- **Remote Backend & State Locking:** Storing `terraform.tfstate` in an encrypted Amazon S3 bucket with a DynamoDB table lock so two engineers cannot run `apply` simultaneously!
- **Terraform Drift:** When someone manually changes a setting in the AWS web console, causing real-world cloud resources to differ from your Git code.
- **Terraform Module:** A reusable folder of Terraform code packaging a complete pattern (e.g., standard production VPC with 4 subnets).

### Sample `main.tf` Architecture:
```hcl
# 1. Required Provider
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  # Remote Backend with State Locking
  backend "s3" {
    bucket         = "my-company-terraform-state"
    key            = "prod/vpc.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
  }
}

# 2. Configure AWS Provider
provider "aws" {
  region = var.aws_region
}

# 3. Create a Resource
resource "aws_s3_bucket" "prod_assets" {
  bucket = "my-prod-unique-bucket-12345"
  tags = {
    Environment = "Production"
    ManagedBy   = "Terraform"
  }
}

# 4. Output the Bucket Domain Name
output "bucket_domain" {
  description = "The website endpoint for the bucket"
  value       = aws_s3_bucket.prod_assets.bucket_regional_domain_name
}
```

---

## 💻 3. Essential Terraform CLI Commands & Screen Output Breakdown

### 1️⃣ The Core Terraform Lifecycle
| Command | Simple 1-Line Definition | What Details Appear on Screen |
| :--- | :--- | :--- |
| **`terraform init`** | Downloads necessary provider plugins (AWS, Azure) and connects to the remote backend. | `Terraform has been successfully initialized!` |
| **`terraform fmt`** | Rewrites Terraform configuration files to canonical format and clean spacing. | Lists filenames that were reformatted. |
| **`terraform validate`** | Verifies that HCL syntax, resource names, and attribute arguments are 100% correct. | `Success! The configuration is valid.` |
| **`terraform plan`** | Compares code against real cloud state and generates an execution preview. | Prints diff: `Plan: 3 to add, 1 to change, 0 to destroy.` |
| **`terraform apply`** | Executes the plan and provisions real-world infrastructure in the cloud. | Prompts `Do you want to perform these actions? (yes/no):` followed by creation progress. |
| **`terraform apply -auto-approve`**| Executes apply instantly without asking for interactive keyboard confirmation (used in CI/CD). | Streams creation logs directly. |
| **`terraform destroy`** | Deletes every single resource tracked inside the state file permanently. | Prompts `Do you really want to destroy all resources?` followed by deletion logs. |

---

### 2️⃣ Understanding `terraform plan` Output Symbols
```text
  # aws_instance.web will be created
  + resource "aws_instance" "web" {
      + ami                          = "ami-0c55b159cbfafe1f0"
      + instance_type                = "t3.micro"
      + id                           = (known after apply)
    }

  # aws_security_group.allow_web will be updated in-place
  ~ resource "aws_security_group" "allow_web" {
      ~ description = "Old description" -> "New production description"
    }

  # aws_s3_bucket.old_backup will be destroyed
  - resource "aws_s3_bucket" "old_backup" {
      - bucket = "temp-backup-bucket" -> null
    }

Plan: 1 to add, 1 to change, 1 to destroy.
```
- `+` (Green): Resource will be newly **created**.
- `~` (Yellow): Resource will be **modified in-place** without downtime.
- `-+` (Red/Green): Resource must be **destroyed and recreated** (downtime warning!).
- `-` (Red): Resource will be permanently **deleted**.

---

### 3️⃣ State Management Commands
- **`terraform state list`**: Lists all resource addresses stored in the current state file.
- **`terraform state show <RESOURCE>`**: Prints full attribute details of a single resource from state.
- **`terraform import <RESOURCE> <REAL_CLOUD_ID>`**: Adopts an existing cloud resource created manually in the console into Terraform management without destroying it!
- **`terraform refresh`**: Queries cloud APIs to update the state file with any real-world changes.

---

## ⚡ 4. Ansible Architecture & Core Concepts

Ansible is an open-source IT automation engine for configuration management, application deployment, and task orchestration.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                          ANSIBLE ARCHITECTURE                          │
│                                                                        │
│   ┌────────────────────┐          ┌────────────────────┐               │
│   │   CONTROL NODE     │          │    INVENTORY       │               │
│   │ (Your laptop / VM) │          │  (hosts.ini / yml) │               │
│   │ Ansible installed  │          │ List of target IPs │               │
│   └─────────┬──────────┘          └─────────▲──────────┘               │
│             │                               │                          │
│             │ SSH (Port 22) / WinRM         │                          │
│             ├───────────────────────────────┘                          │
│             ▼                                                          │
│   ┌────────────────────────────────────────────────────────────┐       │
│   │                     MANAGED WORKER NODES                   │       │
│   │                                                            │       │
│   │   ┌──────────────┐     ┌──────────────┐     ┌──────────┐   │       │
│   │   │  Web Node 1  │     │  Web Node 2  │     │ Database │   │       │
│   │   │ (Zero agents)│     │ (Zero agents)│     │ (Python) │   │       │
│   │   └──────────────┘     └──────────────┘     └──────────┘   │       │
│   └────────────────────────────────────────────────────────────┘       │
└────────────────────────────────────────────────────────────────────────┘
```

### Core Concepts (1-Line Definitions):
- **Agentless:** Ansible requires NO background agent software installed on worker nodes; it connects over standard **SSH** (Linux) or **WinRM** (Windows).
- **Push-Based Architecture:** The control machine connects out and executes commands on remote servers directly, rather than nodes polling a central server.
- **Control Node:** The computer (Linux/macOS) where Ansible is installed and commands are launched from.
- **Managed Node:** Target remote servers configured and managed by the control node (needs only Python installed).
- **Inventory File (`hosts.ini`):** A plain text file defining the IP addresses, domain names, and group categories of all target servers.
- **Playbook (`playbook.yml`):** A YAML file declaring a series of plays and ordered tasks executed on selected server groups.
- **Task:** A single action executed on target nodes (e.g., "Install Nginx package" or "Start MySQL service").
- **Module:** The pre-built worker programs Ansible executes to perform tasks (`apt`, `yum`, `service`, `copy`, `template`, `file`).
- **Handler:** A special task that runs only when notified by another task that made an actual change (e.g., restart Nginx only if the configuration file changed!).
- **Ansible Roles:** A standardized, reusable directory bundle containing tasks, handlers, variables, and templates structured for large enterprise automation.
- **Ansible Vault:** Built-in encryption tool using AES-256 to securely encrypt passwords, SSH keys, and certificates inside Git repositories.

---

## 📜 5. Ansible Inventory, Playbook & Roles Structure

### 1️⃣ Sample Inventory (`hosts.ini`):
```ini
[webservers]
web1.production.internal ansible_host=10.0.1.15
web2.production.internal ansible_host=10.0.1.16

[databases]
db1.production.internal  ansible_host=10.0.2.20

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

---

### 2️⃣ Sample Playbook (`site.yml`):
```yaml
---
- name: Configure Production Web Servers
  hosts: webservers
  become: yes          # Run tasks with sudo root privileges

  vars:
    http_port: 80
    app_dir: /var/www/html

  tasks:
    # Task 1: Install NGINX web server
    - name: Ensure Nginx is installed
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: yes

    # Task 2: Deploy custom configuration template
    - name: Deploy custom Nginx config
      ansible.builtin.template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: '0644'
      notify: Restart Nginx   # Triggers handler only if file was altered!

    # Task 3: Ensure Nginx service is running and enabled on boot
    - name: Ensure Nginx service is running
      ansible.builtin.systemd:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: Restart Nginx
      ansible.builtin.systemd:
        name: nginx
        state: restarted
```

---

### 3️⃣ Standard Ansible Role Directory Tree:
```text
roles/webserver/
├── tasks/
│   └── main.yml        <── Primary list of tasks executed by the role
├── handlers/
│   └── main.yml        <── Handlers notified by tasks
├── templates/
│   └── nginx.conf.j2   <── Dynamic Jinja2 config templates
├── files/
│   └── index.html      <── Static files copied directly to servers
├── vars/
│   └── main.yml        <── High-priority variables for this role
└── defaults/
    └── main.yml        <── Default low-priority fallback variables
```

---

## 💻 6. Essential Ansible CLI Commands & Output Breakdown

### 1️⃣ Ad-Hoc Commands (Quick Fire Actions)
```bash
# Test SSH connectivity and Python availability across all servers
ansible all -i hosts.ini -m ping

# Check server disk space across all webservers
ansible webservers -i hosts.ini -m command -a "df -h"

# Reboot all database servers with sudo
ansible databases -i hosts.ini -b -m reboot
```

- **What details appear on `ansible all -m ping`:**
```json
web1.production.internal | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

---

### 2️⃣ Executing Playbooks
```bash
# 1. Run playbook across target infrastructure
ansible-playbook -i hosts.ini site.yml

# 2. Dry-run test mode (shows what would change without modifying servers)
ansible-playbook -i hosts.ini site.yml --check

# 3. Check syntax errors in YAML playbook
ansible-playbook site.yml --syntax-check

# 4. Limit execution to a single specific host
ansible-playbook -i hosts.ini site.yml --limit web1.production.internal
```

- **What details appear on screen during playbook run:**
```text
PLAY [Configure Production Web Servers] ****************************************

TASK [Gathering Facts] *********************************************************
ok: [web1]
ok: [web2]

TASK [Ensure Nginx is installed] ***********************************************
changed: [web1]
ok: [web2]

TASK [Ensure Nginx service is running] ******************************************
ok: [web1]
ok: [web2]

PLAY RECAP *********************************************************************
web1    : ok=3    changed=1    unreachable=0    failed=0    skipped=0
web2    : ok=3    changed=0    unreachable=0    failed=0    skipped=0
```
- `ok`: Task verified server was already in desired state (idempotent, no action taken).
- `changed`: Task performed a real modification on the target server.
- `failed=0`: All steps succeeded with zero errors.

---

### 3️⃣ Ansible Vault (Encrypting Secrets)
```bash
# 1. Create a brand new encrypted secrets file
ansible-vault create vars/vault.yml

# 2. Encrypt an existing plain text file
ansible-vault encrypt vars/secrets.yml

# 3. Edit an encrypted file in-place (decrypts in RAM, re-encrypts on save)
ansible-vault edit vars/vault.yml

# 4. Run a playbook that uses vaulted secrets
ansible-playbook -i hosts.ini site.yml --ask-vault-pass
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Git & CI/CD Short Notes](./07-Git-and-CICD-Short-Notes.md) | [Index](../README.md) | [AWS Services & Types Short Notes →](./09-AWS-Services-and-Types-Deep-Dive-Short-Notes.md) |
