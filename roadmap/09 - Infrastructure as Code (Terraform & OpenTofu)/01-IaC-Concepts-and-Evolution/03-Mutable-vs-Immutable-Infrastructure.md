# 03 - Mutable vs Immutable Infrastructure

## 1. The Two Models

```text
MUTABLE INFRASTRUCTURE                 IMMUTABLE INFRASTRUCTURE
────────────────────────               ─────────────────────────
Server deployed ──► SSH in ──►         Build golden image ──►
Patch packages ──► Config drift ──►    Deploy fresh VM/container ──►
More patches ──► "Snowflake" server    Destroy old instance ──►
                                       No SSH, no drift
```

### Snowflake Server Anti-Pattern
A **snowflake server** is a machine that has been manually configured so many times that no one knows its exact state. Rebuilding it from scratch is considered impossible.

### Phoenix Server Pattern
A **phoenix server** rises from the ashes — it's destroyed and rebuilt from code on every deployment. The IaC definition is the single source of truth.

## 2. Configuration Drift

| Aspect | Mutable | Immutable |
|---|---|---|
| **Update Method** | In-place patches (apt upgrade, yum update) | Replace entire instance with new image |
| **Drift Risk** | High — manual changes accumulate over time | Zero — instances are never modified post-deployment |
| **Rollback** | Complex — must reverse individual changes | Simple — deploy previous image version |
| **State Consistency** | Diverges over time across fleet | Identical across all instances |
| **Tools** | Ansible, Chef, Puppet (configuration management) | Packer + Terraform, Docker + Kubernetes |
| **SSH Access** | Required for maintenance | Disabled entirely (no shell access) |

## 3. The Golden Image Pipeline

```text
Source Code ──► Packer Build ──► AMI / VM Image ──► Image Registry
                    │                                     │
                    │ (install packages, harden OS,       │
                    │  bake configs, run tests)            │
                    │                                     ▼
                    └──────────────────────► Terraform Deploy
                                              (Launch from image)
```

### Packer Template Example
```json
{
  "builders": [{
    "type": "amazon-ebs",
    "source_ami": "ami-base-ubuntu-22.04",
    "instance_type": "t3.micro",
    "ami_name": "web-server-{{timestamp}}"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl enable nginx"
    ]
  }]
}
```

## 4. Real-World Decision Matrix

| Scenario | Recommended Approach |
|---|---|
| Kubernetes workloads | Immutable (container images, rolling updates) |
| Legacy monolith on bare metal | Mutable (Ansible for config management) |
| Auto Scaling Groups (ASGs) | Immutable (launch from AMI/image) |
| Database servers | Hybrid (immutable OS image + persistent data volumes) |
| Emergency security patches | Mutable (patch in-place) + Immutable (rebuild image for future) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - IaC Tooling Landscape Terraform Pulumi CloudFormation](./02-IaC-Tooling-Landscape-Terraform-Pulumi-CloudFormation.md) | [Index](../../../README.md) | [04 - IaC Workflow Write Plan Apply →](./04-IaC-Workflow-Write-Plan-Apply.md) |
