# 05 - Packer Image Baking vs. Runtime Provisioning

In high-scale cloud production environments, provisioning infrastructure efficiently requires choosing when and where software dependencies are installed. Infrastructure architects must decide between **Build-Time Baking** (compiling complete, immutable machine images with HashiCorp Packer before deployment) and **Run-Time Provisioning** (installing software on instances as they boot using Terraform provisioners, cloud-init, or Ansible).

---

## 1. The Architectural Paradigm: Bake vs. Fry

Cloud engineering uses the culinary metaphor of "Baking vs. Frying" to define where machine configuration occurs:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      THE "BAKE VS. FRY" SPECTRUM                                │
│                                                                                 │
│   PURE BAKE (Golden Image)          HYBRID (Bake + Fry)       PURE FRY (Runtime)│
│                                                                                 │
│   ┌───────────────────────┐       ┌───────────────────┐     ┌─────────────────┐ │
│   │ Everything Pre-Baked  │       │ OS + Base Tools   │     │ Vanilla Base OS │ │
│   │ App Code, Binaries,   │       │ Pre-Baked; Config │     │ Everything      │ │
│   │ Security Agents, Libs │       │ Injected at Boot  │     │ Installed at    │ │
│   └───────────┬───────────┘       └─────────┬─────────┘     │ Boot via Script │ │
│               │                             │               └────────┬────────┘ │
│       Boot: 20 Seconds              Boot: 2 Minutes                  │          │
│       Zero External Deps            Dynamic Envs             Boot: 12 Minutes   │
│       Highest Reliability           Balanced Scalability     High Drift Risk    │
│               │                             │                        │          │
│               ▼                             ▼                        ▼          │
│     Auto-Scaling Fleets             Enterprise SaaS         Dev / Scratch VMs   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The SRE Cold-Start Crisis: Why Runtime Provisioning Fails in Production

Consider an Auto Scaling Group (ASG) experiencing a sudden 500% traffic surge (e.g., Black Friday flash sale):
- **Runtime Provisioning Approach:** When the ASG launches 20 new EC2 instances, each node executes `apt-get update`, pulls hundreds of megabytes of packages from public mirrors, clones Git repositories, and compiles assets.
  - **Result:** Instance boot time takes **10 to 15 minutes**.
  - **Failure Modes:** Upstream package mirror rate-limits or network timeouts cause 3 out of 20 instances to fail bootstrap, leaving the cluster under-provisioned and triggering 502/504 Bad Gateway gateway outages.
- **Packer Golden Image Approach:** All operating system packages, runtimes, security agents, and systemd service units are compiled into a pre-tested Amazon Machine Image (AMI).
  - **Result:** Instance boot time takes **25 to 45 seconds**. The instances immediately pass load balancer target health checks with zero external network dependencies.

---

## 2. Comprehensive Comparison Matrix

| Architectural Metric | Packer Image Baking (Build-Time) | Runtime Provisioners / Cloud-Init |
| :--- | :--- | :--- |
| **Boot Speed** | **Fastest (20–45 seconds)** | Slow (5–15 minutes) |
| **Deployment Predictability** | **100% Deterministic** (Identical bits across all nodes) | Non-deterministic (Upstream mirrors may update or fail) |
| **Security Scanning** | **Static Shift-Left** (Vulnerabilities scanned before release) | Dynamic (Vulnerabilities discovered in production) |
| **Auto-Scaling Suitability** | **Mission-Critical / Production Grade** | Unsuitable for real-time traffic spikes |
| **Configuration Drift** | **Zero Drift** (Nodes are discarded and replaced) | High Drift (In-place changes cause node variance) |
| **Pipeline Complexity** | Requires separate CI/CD image build pipeline | Single-step Terraform execution |
| **Cost Efficiency** | Minimal wasted compute during startup | High compute costs spent compiling on running VMs |

---

## 3. Production Golden Image Pipeline Architecture

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      ENTERPRISE GOLDEN IMAGE PIPELINE                           │
│                                                                                 │
│  1. Code Commit      2. Packer Builder      3. Provisioning   4. Vulnerability  │
│  (HCL2 / Ansible)    (EBS Clean Runner)     (CIS Hardening)      (Trivy / CIS)  │
│                                                                                 │
│   ┌──────────────┐     ┌──────────────┐     ┌─────────────┐     ┌─────────────┐ │
│   │ Git Push     │────►│ HashiCorp    │────►│ Ansible     │────►│ Inspector / │ │
│   │ packer.pkr.hcl│    │ Packer Build │     │ Playbook    │     │ Trivy Scan  │ │
│   └──────────────┘     └──────┬───────┘     └─────────────┘     └──────┬──────┘ │
│                               │                                        │        │
│                               ▼                                        ▼        │
│                      ┌─────────────────┐                      ┌─────────────────┐
│                      │ Encrypted AMI   │                      │ Security Gate:  │
│                      │ Generated       │                      │ 0 Critical CVEs │
│                      └────────┬────────┘                      └────────┬────────┘
│                               │                                        │        │
│                               ▼                                        ▼        │
│                      ┌────────────────────────────────────────────────────────┐ │
│                      │ 5. Terraform Deployment: ASG Instance Refresh (Zero DT)│ │
│                      └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Production Code Implementations

### Step 1: The Modern Packer HCL2 Template (`ubuntu-golden.pkr.hcl`)

```hcl
packer {
  required_plugins {
    amazon = {
      version = ">= 1.2.8"
      source  = "github.com/hashicorp/amazon"
    }
    ansible = {
      version = ">= 1.1.0"
      source  = "github.com/hashicorp/ansible"
    }
  }
}

variable "aws_region" {
  type    = string
  default = "us-east-1"
}

variable "environment" {
  type    = string
  default = "production"
}

locals {
  timestamp = regex_replace(timestamp(), "[- TZ:]", "")
}

# Define the source base OS AMI (Canonical Ubuntu 22.04 LTS)
source "amazon-ebs" "hardened_node" {
  region        = var.aws_region
  instance_type = "c6i.large"
  ami_name      = "golden-node-${var.environment}-${local.timestamp}"

  source_ami_filter {
    filters = {
      name                = "ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    most_recent = true
    owners      = ["099720109477"] # Canonical official account
  }

  ssh_username = "ubuntu"
  communicator = "ssh"

  # Encrypt root volume with customer-managed KMS key
  encrypt_boot = true

  tags = {
    Name        = "golden-node-${var.environment}-${local.timestamp}"
    BaseAMI     = "{{ .SourceAMI }}"
    Environment = var.environment
    ManagedBy   = "Packer"
  }
}

build {
  sources = ["source.amazon-ebs.hardened_node"]

  # Run Ansible playbook inside the temporary builder VM
  provisioner "ansible" {
    playbook_file = "./ansible/site.yml"
    extra_arguments = [
      "--extra-vars", "target_env=${var.environment}"
    ]
  }

  # Clean up machine identifiers and SSH host keys before snapshotting
  provisioner "shell" {
    inline = [
      "sudo rm -rf /tmp/* /var/tmp/*",
      "sudo truncate -s 0 /etc/machine-id",
      "sudo rm -f /etc/ssh/ssh_host_*",
      "sudo cloud-init clean --logs"
    ]
  }
}
```

---

### Step 2: Consuming the Golden AMI in Terraform with Zero-Downtime Rolling Update

```hcl
# Fetch the latest validated Golden AMI built by Packer
data "aws_ami" "golden_app" {
  most_recent = true
  owners      = ["self"]

  filter {
    name   = "name"
    values = ["golden-node-production-*"]
  }

  filter {
    name   = "state"
    values = ["available"]
  }
}

# Launch Template points directly to the pre-baked AMI
resource "aws_launch_template" "api_cluster" {
  name_prefix   = "api-worker-"
  image_id      = data.aws_ami.golden_app.id
  instance_type = "c6i.large"

  iam_instance_profile {
    name = aws_iam_instance_profile.app_profile.name
  }

  # Minimal user_data: Only injects dynamic runtime environment variables!
  user_data = base64encode(<<-EOF
    #!/bin/bash
    echo "DATABASE_URL=${aws_db_instance.postgres.endpoint}" > /etc/app.env
    systemctl restart api-service
  EOF
  )

  lifecycle {
    create_before_destroy = true
  }
}

# Auto Scaling Group executes an automated rolling instance refresh
resource "aws_autoscaling_group" "api_asg" {
  name_prefix         = "production-api-asg-"
  vpc_zone_identifier = aws_subnet.private[*].id
  target_group_arns   = [aws_lb_target_group.api.arn]
  min_size            = 3
  max_size            = 12
  desired_capacity    = 3

  launch_template {
    id      = aws_launch_template.api_cluster.id
    version = "$Latest"
  }

  # Automated Rolling Deployment when AMI updates!
  instance_refresh {
    strategy = "Rolling"
    preferences {
      min_healthy_percentage = 100
      instance_warmup        = 300
    }
    triggers = ["tag"]
  }
}
```

---

## 5. Enterprise Best Practices Checklist

1. **Keep `user_data` Under 10 Lines:** In a mature cloud architecture, `user_data` should only write environment secrets and start services. If your `user_data` is installing compilers or running `apt-get upgrade`, bake those tools into Packer.
2. **Always Sanitize the Golden Image:** Strip out cloud-init cache, SSH authorized keys, and bash history (`/etc/machine-id`, `/root/.bash_history`) before the AMI snapshot is created.
3. **Automate AMI Retention and Deregistration:** Implement automated lifecycle policies (e.g., AWS Image Builder or AWS Systems Manager automation) to deregister and delete EBS snapshots older than 90 days.
4. **Enforce Encryption at Rest:** Never create unencrypted golden AMIs. Ensure Packer builder configurations enable `encrypt_boot = true` using enterprise AWS KMS Customer Managed Keys (CMKs).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - User Data and Cloud Init vs Provisioners](./04-User-Data-and-Cloud-Init-vs-Provisioners.md) | [Index](../../../README.md) | [06 - External Data Source and Custom Scripts →](./06-External-Data-Source-and-Custom-Scripts.md) |

