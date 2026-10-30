# 03 - Mutable vs Immutable Infrastructure

## 1. The Core Paradigm: "Pets vs. Cattle"

The architectural debate between **Mutable** and **Immutable** infrastructure is often summarized by the famous industry paradigm coined by Randy Bias: **"Pets vs. Cattle"**.

- **Pets (Mutable Infrastructure):** Servers are treated as unique, precious animals. Each machine has a personal hostname (e.g., `zeus`, `hermes`, `db-master`), is lovingly cared for by sysadmins, and when it falls ill, engineers spend hours diagnosing kernel logs, running `apt-get upgrade`, and nursing it back to health.
- **Cattle (Immutable Infrastructure):** Servers are treated as identical, numbered livestock in a commercial herd (e.g., `web-asg-001`, `web-asg-002`). They are completely fungible and unattached to sentiment. If a server becomes unresponsive or requires a software patch, engineers **do not repair it**; they destroy it immediately and launch a brand-new, pre-baked, healthy instance in its place.

```text
+---------------------------------------------------------------------------------------------------+
|                        MUTABLE ("PETS") VS IMMUTABLE ("CATTLE") PARADIGM                          |
+---------------------------------------------------------------------------------------------------+

 MUTABLE INFRASTRUCTURE (IN-PLACE REPAIR - "PETS"):
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Server VM (Running for 3 Years)                                                  │
 │   1. Vulnerability detected ──► Engineer SSHs into server                        │
 │   2. Runs "apt-get upgrade" ──► Halfway through, network drops                    │
 │   3. Package manager locks (/var/lib/dpkg/lock) ──► Library versions mismatch!   │
 │   4. Server enters undefined, drifted state ("Snowflake Server")                 │
 └──────────────────────────────────────────────────────────────────────────────────┘

 IMMUTABLE INFRASTRUCTURE (REPLACE & DESTROY - "CATTLE"):
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Golden Image Pipeline (Packer + Terraform)                                       │
 │   1. New security patch ──► CI/CD pipeline bakes a brand-new Golden AMI          │
 │   2. Image passes automated CVE scanning & smoke testing in isolation           │
 │   3. Terraform provisions NEW instances running the new image                    │
 │   4. Load balancer routes traffic to new instances ──► Old instances TERMINATED! │
 │   5. Result: Zero configuration drift, zero SSH access required!                 │
 └──────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. In-Depth Comparison: Mutable vs. Immutable Models

| Operational Vector | Mutable Infrastructure (In-Place) | Immutable Infrastructure (Replace-Only) |
| :--- | :--- | :--- |
| **How Updates Are Applied** | In-place modifications via SSH, Ansible, Puppet, or Chef. | Destroy old server; launch a brand-new VM image from a pre-baked template. |
| **Configuration Drift Risk** | **Severe:** Over time, manual hotfixes and package inconsistencies create snowflake instances. | **Zero:** Disks are read-only or ephemeral; instances are never modified after boot. |
| **Rollback Velocity** | **Slow and complex:** Must write reverse playbooks to downgrade packages and revert database configs. | **Instant:** Update the AMI ID in Terraform back to the previous hash and redeploy. |
| **Testing Confidence** | **Low:** Changes are tested on live production servers; failure modes can corrupt the OS. | **100% Deterministic:** The exact byte-for-byte image deployed in production was pre-tested in staging. |
| **SSH / Shell Access** | Required for sysadmins to perform ongoing operational maintenance. | **Completely disabled:** No port 22 open; debugged exclusively via centralized logs and metrics. |
| **Tooling Ecosystem** | Ansible, Puppet, Chef, SaltStack, Bash. | HashiCorp Packer, Terraform, OpenTofu, Docker, Kubernetes. |

---

## 3. The 5-Phase Golden Image & Deployment Pipeline

In enterprise cloud architectures, immutable infrastructure is realized through a synchronized pipeline connecting **HashiCorp Packer** (the image builder) with **Terraform/OpenTofu** (the infrastructure provisioner):

```text
+---------------------------------------------------------------------------------------------------+
|                        THE END-TO-END IMMUTABLE DEPLOYMENT PIPELINE                               |
+---------------------------------------------------------------------------------------------------+

 1. BAKE PHASE (HashiCorp Packer)
    Takes a clean base OS (Ubuntu 22.04 LTS), installs runtime dependencies (Node.js/JVM),
    applies CIS security benchmarks, pre-compiles application code, and outputs a Golden AMI:
    "ami-09c8e1a42b10f"
           │
           ▼
 2. SECURITY SCANNING PHASE (Automated DevSecOps)
    CI/CD runs Trivy / AWS Inspector against the Golden AMI to guarantee zero high-severity CVEs.
           │
           ▼
 3. PROVISIONING PHASE (Terraform / OpenTofu)
    Terraform references the new Golden AMI ID in an AWS Launch Template / Auto Scaling Group:
    image_id = "ami-09c8e1a42b10f"
           │
           ▼
 4. BLUE/GREEN TRAFFIC CUTOVER (Application Load Balancer)
    New instances boot and pass health checks. ALB gradually shifts user traffic:
    Old ASG (Blue): 100% ──► 50% ──► 0%
    New ASG (Green):  0% ──► 50% ──► 100%
           │
           ▼
 5. TERMINATION PHASE (Garbage Collection)
    Terraform terminates all old Blue instances. Total downtime: 0 seconds!
```

---

## 4. Production Golden Image Definition (Packer Modern HCL2 Syntax)

Modern enterprise environments use Packer's **HCL2 syntax** (deprecating legacy JSON) to declare immutable virtual machine images:

```hcl
# File: golden-image.pkr.hcl
packer {
  required_plugins {
    amazon = {
      version = ">= 1.2.8"
      source  = "github.com/hashicorp/amazon"
    }
  }
}

variable "app_version" {
  type    = string
  default = "2.4.1"
}

source "amazon-ebs" "ubuntu_base" {
  ami_name      = "enterprise-golden-image-v${var.app_version}-${formatdate("YYYYMMDDhhmmss", timestamp())}"
  instance_type = "c6i.large"
  region        = "us-east-1"
  
  # Source Base: Official Canonical Ubuntu 22.04 LTS AMI
  source_ami_filter {
    filters = {
      name                = "ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    most_recent = true
    owners      = ["099720109477"] # Canonical AWS Account ID
  }
  
  ssh_username = "ubuntu"
}

build {
  name    = "bake-web-tier"
  sources = ["source.amazon-ebs.ubuntu_base"]

  # Hardening and provisioning scripts
  provisioner "shell" {
    inline = [
      "sudo apt-get update && sudo apt-get upgrade -y",
      "sudo apt-get install -y nginx curl jq fail2ban",
      # Apply CIS OS Benchmark hardening
      "sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config",
      # Install Application Artifact
      "curl -sL https://artifactory.internal/apps/release-${var.app_version}.tar.gz | sudo tar -xz -C /var/www/html",
      "sudo systemctl enable nginx"
    ]
  }
}
```

---

## 5. Deploying the Immutable Image with Terraform

Once the Golden Image is baked and tagged in the cloud image registry, Terraform provisions an Auto Scaling Group utilizing an instance refresh policy to perform zero-downtime rolling replacement:

```hcl
# File: compute.tf
data "aws_ami" "latest_golden" {
  most_recent = true
  owners      = ["self"]

  filter {
    name   = "name"
    values = ["enterprise-golden-image-v*"]
  }
}

resource "aws_launch_template" "web" {
  name_prefix   = "web-tier-"
  image_id      = data.aws_ami.latest_golden.id
  instance_type = "t3.medium"

  # Zero SSH Keys configured: Access is granted exclusively via AWS SSM Session Manager
  iam_instance_profile {
    name = "AmazonSSMManagedInstanceCoreProfile"
  }

  tag_specifications {
    resource_type = "instance"
    tags = {
      Environment = "Production"
      ManagedBy   = "Terraform"
    }
  }
}

resource "aws_autoscaling_group" "web_asg" {
  name_prefix         = "web-asg-"
  desired_capacity    = 4
  max_size            = 8
  min_size            = 2
  vpc_zone_identifier = var.private_subnet_ids

  launch_template {
    id      = aws_launch_template.web.id
    version = "$Latest"
  }

  # Automated Rolling Replacement Policy
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

## 6. SRE Operational Best Practices for Immutable Fleets

1. **Eliminate Port 22 Completely:** Never open inbound TCP port 22 in production security groups. When troubleshooting is unavoidable, use **AWS Systems Manager (SSM) Session Manager** or **Azure Bastion**, which log every command executed to a secure S3 bucket with full audit attribution.
2. **Short-Lived Instances (Ephemeral Lifecycles):** Periodically cycle instances even when no software update has occurred (e.g., maximum instance lifetime of 14 days). This guarantees that memory leaks and transient file caches are automatically purged.
3. **Decouple Ephemeral Compute from Persistent Data:** Never store persistent state (such as customer uploads or SQLite databases) on the root filesystem of an immutable VM. Always offload state to dedicated persistent cloud services: Amazon S3, Google Cloud Storage, or managed database clusters (Amazon RDS / Aurora).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - IaC Tooling Landscape Terraform Pulumi CloudFormation](./02-IaC-Tooling-Landscape-Terraform-Pulumi-CloudFormation.md) | [Index](../../../README.md) | [04 - IaC Workflow Write Plan Apply →](./04-IaC-Workflow-Write-Plan-Apply.md) |
