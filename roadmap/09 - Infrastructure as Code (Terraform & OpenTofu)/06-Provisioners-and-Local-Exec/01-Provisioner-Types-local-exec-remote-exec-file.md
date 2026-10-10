# 01 - Provisioner Types: local-exec, remote-exec & file

Provisioners in HashiCorp Terraform and OpenTofu provide an imperative escape hatch inside a fundamentally declarative infrastructure model. While declarative HCL describes **what** end state should exist in the cloud, provisioners execute shell commands, copy files, or trigger remote configuration engines to perform custom bootstrap tasks that cannot be accomplished through native cloud APIs. Because provisioners introduce external side effects that bypass state tracking, they are officially classified as a **last resort** mechanism.

---

## 1. Why Provisioners Exist (and Why HashiCorp Classifies Them as a Last Resort)

In a pure declarative paradigm, the infrastructure orchestrator issues API calls and records the returned identifiers inside `terraform.tfstate`. The orchestrator understands resource attributes, dependencies, and lifecycles. 

Provisioners break this contract by executing arbitrary external scripts outside the purview of the Terraform engine:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    DECLARATIVE VS IMPERATIVE PROVISIONING                       │
│                                                                                 │
│   DECLARATIVE NATIVE RESOURCES                  IMPERATIVE PROVISIONERS         │
│   (aws_instance, aws_vpc)                       (local-exec, remote-exec)       │
│                                                                                 │
│   ┌───────────────────────────┐                 ┌───────────────────────────┐   │
│   │ Terraform Core & Provider │                 │ External Shell / SSH Sub  │   │
│   │ API Reconciliation Engine │                 │ Execution Process         │   │
│   └─────────────┬─────────────┘                 └─────────────┬─────────────┘   │
│                 │                                             │                 │
│         Idempotent Sync                                Non-Idempotent Run       │
│         State Tracked (Diff)                           Zero State Tracking      │
│         Plan Visibility: YES                           Plan Visibility: NONE    │
│                 │                                             │                 │
│                 ▼                                             ▼                 │
│   ┌───────────────────────────┐                 ┌───────────────────────────┐   │
│   │ Cloud Infrastructure API  │                 │ Unmanaged Local / VM State│   │
│   │ (AWS, Azure, GCP Console) │                 │ (Bash, Files, apt packages│   │
│   └───────────────────────────┘                 └───────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The 4 Major Operational Dangers of Provisioners

1. **Invisible During `terraform plan`:** Terraform cannot predict or dry-run what a Bash or PowerShell command will execute. The execution plan merely shows that a resource will be created or modified; it provides zero visibility into whether the provisioner script will succeed, fail, or delete files.
2. **Loss of Idempotency:** Native Terraform resource updates are strictly idempotent. A provisioner script running `curl` or `apt-get` might fail on the second execution if files already exist, networks drop, or package versions shift upstream.
3. **The "Tainted Resource" Trap:** If a provisioner fails during resource creation, Terraform marks the entire resource as **tainted** (or schedules it for recreation in modern Terraform). On the subsequent `terraform apply`, Terraform will destroy the running virtual machine and recreate it from scratch, causing catastrophic production outages.
4. **Network and Security Friction:** Running `remote-exec` requires opening inbound SSH (port 22) or WinRM (port 5986) ports from the CI/CD runner to the target server, distributing private SSH keys, and managing firewall rules that violate zero-trust architecture.

### The Architectural Decision Matrix

```text
                            PROVISIONER DECISION FLOW
                            ─────────────────────────
Are you trying to configure software inside a virtual machine?
├── YES ──► Can the configuration be baked into a custom image?
│           ├── YES ──► Use Packer to build a Golden AMI / VHD (Best Practice)
│           └── NO  ──► Can you pass bootstrapping scripts at boot time?
│                       ├── YES ──► Use cloud-init or EC2 user_data (Cloud Native)
│                       └── NO  ──► Use Systems Manager (AWS SSM / Azure Run Command)
└── NO  ──► Are you invoking local CLI tools (e.g., local file generation, Kubernetes Helm)?
            ├── YES ──► Check if an official Terraform Provider exists (e.g., local, helm, kubernetes)
            └── NO  ──► Only now use a Provisioner (local-exec / remote-exec) as a LAST RESORT
```

---

## 2. In-Depth Breakdown of the Three Built-In Provisioners

Terraform includes three native provisioner plugins embedded within the Core engine.

### 1️⃣ `local-exec` (Executes Commands on the Machine Running Terraform)

The `local-exec` provisioner invokes a process locally on the developer's laptop, bastion host, or CI/CD agent (e.g., GitHub Actions runner) executing the `terraform apply` command.

```hcl
resource "aws_instance" "app_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.medium"
  subnet_id     = aws_subnet.private_app.id

  tags = {
    Name        = "production-api-01"
    Environment = "production"
  }

  # Write the newly provisioned IP address to a local Ansible inventory
  provisioner "local-exec" {
    command     = "echo '${self.private_ip} ansible_user=ubuntu' >> ./inventories/hosts.ini"
    working_dir = path.root
    interpreter = ["/bin/bash", "-c"]

    environment = {
      DEPLOYMENT_ENV = "production"
      INSTANCE_ID    = self.id
    }
  }
}
```

#### Key `local-exec` Parameters:
- `command`: The raw shell string or script path to execute.
- `working_dir`: The directory relative to the module root where the command runs.
- `interpreter`: A list containing the shell binary and arguments (e.g., `["/bin/bash", "-c"]` on Linux or `["powershell", "-Command"]` on Windows). If omitted, defaults to `sh` on Unix and `cmd.exe` on Windows.
- `environment`: A map of key-value environment variables passed to the child process.

---

### 2️⃣ `remote-exec` (Executes Commands on the Target Machine via Network)

The `remote-exec` provisioner establishes an interactive network session (SSH or WinRM) from the Terraform execution host directly into the newly provisioned remote server and runs inline or scripted commands.

```hcl
resource "aws_instance" "bastion" {
  ami                         = "ami-0c55b159cbfafe1f0"
  instance_type               = "t3.micro"
  associate_public_ip_address = true
  vpc_security_group_ids      = [aws_security_group.bastion_ssh.id]
  key_name                    = aws_key_pair.deployer.key_name

  # Connection block defines HOW Terraform authenticates to the remote target
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("${path.module}/keys/id_ed25519")
    host        = self.public_ip
    timeout     = "5m"
    agent       = false
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update -y",
      "sudo apt-get install -y fail2ban unattended-upgrades",
      "sudo systemctl enable --now fail2ban",
      "echo 'Bastion host security baseline applied successfully' > /tmp/bootstrap.log"
    ]
  }
}
```

#### Modes of `remote-exec`:
1. `inline`: A list of strings executed sequentially as separate commands.
2. `script`: A local path to a shell script uploaded to a temporary folder on the remote host and executed.
3. `scripts`: A list of multiple local script paths uploaded and executed in order.

---

### 3️⃣ `file` (Transfers Files or Directories to the Target Machine)

The `file` provisioner securely copies files, directories, or raw text strings from the local machine running Terraform into the remote server before configuration scripts execute.

```hcl
resource "aws_instance" "web_worker" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.small"

  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("${path.module}/keys/id_ed25519")
    host        = self.public_ip
  }

  # Scenario A: Copy a single static config file
  provisioner "file" {
    source      = "${path.module}/configs/nginx.conf"
    destination = "/tmp/nginx.conf"
  }

  # Scenario B: Generate dynamic content in HCL and upload directly
  provisioner "file" {
    content     = templatefile("${path.module}/templates/app.env.tftpl", {
      db_endpoint = aws_db_instance.postgres.endpoint
      redis_host  = aws_elasticache_cluster.redis.cache_nodes[0].address
    })
    destination = "/etc/app/environment"
  }

  # Scenario C: Copy an entire directory recursively
  provisioner "file" {
    source      = "${path.module}/scripts/healthcheck/"
    destination = "/opt/scripts/healthcheck"
  }
}
```

---

## 3. Provisioner Execution Lifecycle & Error Behaviors

Understanding when and how provisioners execute prevents accidental downtime and broken pipeline runs.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       PROVISIONER EXECUTION LIFECYCLE                           │
│                                                                                 │
│   1. CREATION-TIME (Default)                 2. DESTROY-TIME (when = destroy)   │
│                                                                                 │
│   ┌───────────────────────────┐              ┌───────────────────────────┐      │
│   │ Resource Created in Cloud │              │ terraform destroy triggered│      │
│   └─────────────┬─────────────┘              └─────────────┬─────────────┘      │
│                 ▼                                          ▼                    │
│   ┌───────────────────────────┐              ┌───────────────────────────┐      │
│   │ Run Provisioner Script    │              │ Run Destroy-Time Script   │      │
│   └─────────────┬─────────────┘              │ (Deregister agent / drain)│      │
│                 │                            └─────────────┬─────────────┘      │
│         ┌───────┴───────┐                                  ▼                    │
│         │               │                    ┌───────────────────────────┐      │
│      SUCCESS         FAILURE                 │ Destroy Cloud Resource    │      │
│         │               │                    └───────────────────────────┘      │
│         ▼               ▼                                                       │
│   ┌───────────┐   ┌────────────────────────┐                                    │
│   │ Recorded  │   │ on_failure = fail      │                                    │
│   │ in State  │   │ -> Resource Tainted    │                                    │
│   └───────────┘   │ (Recreated next apply) │                                    │
│                   │                        │                                    │
│                   │ on_failure = continue  │                                    │
│                   │ -> Marked Clean        │                                    │
│                   └────────────────────────┘                                    │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Creation-Time vs. Destroy-Time Provisioners

- **Creation-Time Provisioners (Default):** Run immediately after the cloud API returns success during resource creation. If the resource already exists in state and is unchanged, the provisioner **never runs again**.
- **Destroy-Time Provisioners (`when = destroy`):** Run **before** the physical cloud resource is deleted from the cloud provider. Commonly used to cleanly drain traffic, deregister nodes from a third-party monitoring cluster (Datadog, Consul), or release floating IP licenses.

```hcl
resource "aws_instance" "consul_node" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  # Destroy-time execution: Gracefully leave the Consul consensus cluster
  provisioner "remote-exec" {
    when = destroy

    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("${path.module}/keys/id_ed25519")
      host        = self.public_ip
    }

    inline = [
      "consul leave",
      "sudo systemctl stop consul"
    ]
  }
}
```

> [!WARNING]
> **Destroy-Time Provisioner Restriction:** During destroy time, the provisioner can **only** inspect attributes of `self`. It **cannot** reference attributes from any other resource (e.g., `aws_security_group.sg.id`), because those resources might have already been scheduled for destruction or altered in the dependency graph.

### Error Handling Policies: `on_failure`

By default, any non-zero exit code returned by a provisioner causes Terraform to abort execution and register an error.

```hcl
resource "aws_instance" "worker" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  provisioner "local-exec" {
    command    = "python3 verify_deployment.py --ip ${self.private_ip}"
    on_failure = continue   # Continues apply without marking instance as tainted
  }
}
```

| Setting | Behavior on Script Failure (Exit Code != 0) | Impact on Terraform State |
| :--- | :--- | :--- |
| `on_failure = fail` *(Default)* | Aborts the current `apply` run immediately. | Marks the resource as **tainted** (or scheduled for recreation). The resource will be destroyed and replaced on next apply. |
| `on_failure = continue` | Logs a warning to the console and proceeds with remaining resources. | Resource is recorded as successfully created in `terraform.tfstate`. |

---

## 4. Modern Cloud-Native Alternatives to Provisioners

Senior infrastructure engineers replace provisioners with cloud-native, declarative, and immutable alternatives:

| Use Case | Legacy Anti-Pattern (Provisioner) | Modern Cloud-Native Alternative | Why the Modern Approach Wins |
| :--- | :--- | :--- | :--- |
| **Installing Web Server / Agents** | `remote-exec` with `apt-get install nginx` | **Packer Image Baking (Golden AMI)** | VMs boot in 15 seconds instead of 10 minutes; zero failure risk from external repository outages. |
| **Dynamic Host Configuration** | `remote-exec` SSH scripts | **Cloud-init / EC2 `user_data`** | Native hypervisor bootstrapping; requires zero inbound SSH ports open to CI/CD runners. |
| **Generating Local Config Files** | `local-exec` echo to file | **`local_file` Provider Resource** | Fully declarative, tracked in state, shows exact line diffs in `terraform plan`. |
| **Database Migrations** | `local-exec` running `alembic upgrade head` | **Kubernetes Job or AWS ECS RunTask** | Isolated execution inside secure VPC boundary with native retries and secrets injection. |
| **Remote Orchestration** | `remote-exec` over SSH | **AWS Systems Manager (SSM) Run Command** | Zero open ports, IAM role-based authentication, complete CloudTrail audit logging. |

---

## 5. Summary and SRE Best Practices

1. **Avoid `remote-exec` in Enterprise Pipelines:** Requiring direct network access from CI/CD runners to internal server subnets violates security boundaries. Use EC2 `user_data` or SSM.
2. **Prefer Native Providers:** Always check if a native provider exists before reaching for `local-exec` (e.g., use the `null` / `external` / `local` / `tls` / `http` providers).
3. **Always Quote and Escape Variables in Shell Scripts:** Prevent shell injection vulnerabilities by properly wrapping `${self.private_ip}` in double quotes.
4. **Isolate Provisioners in Decoupled Resources:** When imperative tasks must be executed, bind them to a `terraform_data` resource rather than attaching them directly to critical production databases or virtual machines to prevent accidental resource recreation.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Terraform-Modules-and-Reusable-Design)](../05-Terraform-Modules-and-Reusable-Design/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Connection Blocks SSH and WinRM →](./02-Connection-Blocks-SSH-and-WinRM.md) |

