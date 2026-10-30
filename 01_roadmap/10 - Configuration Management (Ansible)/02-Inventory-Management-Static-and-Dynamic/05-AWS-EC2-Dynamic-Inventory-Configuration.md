# 05 - AWS EC2 Dynamic Inventory Configuration (`amazon.aws.aws_ec2`)

In modern cloud environments, virtual machines are ephemeral. Auto Scaling Groups dynamically launch and terminate instances, spot instances rotate, and private IP addresses constantly shift. Maintaining static inventory files in this environment causes deployment failures. The **`amazon.aws.aws_ec2`** dynamic inventory plugin queries the AWS DescribeInstances API at runtime to build a real-time, categorized topology of your infrastructure.

---

## 1. Plugin Architecture & Authentication Flow

Unlike legacy pre-Ansible 2.8 executable Python scripts (`ec2.py`), modern inventory plugins are built into Ansible Collections and parse declarative YAML configurations:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      AWS EC2 DYNAMIC INVENTORY WORKFLOW                         │
│                                                                                 │
│   1. Read Config:      2. Authenticate to AWS:     3. Query EC2 Fleet:          │
│   aws_ec2.yml          STS AssumeRole / IRSA       DescribeInstances(running)   │
│   ┌──────────────┐     ┌──────────────────────┐    ┌────────────────────────┐   │
│   │ aws_ec2.yml  │────►│ AWS IAM / Boto3      │───►│ AWS EC2 API Endpoint   │   │
│   └──────────────┘     └──────────────────────┘    └───────────┬────────────┘   │
│                                                                │                │
│   5. Construct Groups: 4. Categorize Metadata:                 ▼                │
│   env_production       keyed_groups: Tags.Env      Parse JSON instance payload  │
│   role_webserver       compose: ansible_host       (InstanceId, PrivateIp, Tags)│
└─────────────────────────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **Strict File Naming Rule:** For Ansible to automatically recognize and invoke the AWS plugin, the inventory file **MUST** end with `aws_ec2.yml` or `aws_ec2.yaml` (e.g., `production_aws_ec2.yml`).

---

## 2. Production `aws_ec2.yml` Configuration

Below is a hardened production configuration incorporating multi-region discovery, tag filtering, dynamic group creation, and custom SSH host mapping:

```yaml
# ==============================================================================
# inventories/production/production_aws_ec2.yml
# ==============================================================================
plugin: amazon.aws.aws_ec2

# Specify target AWS regions
regions:
  - us-east-1
  - us-west-2

# IAM Cross-Account Role Assumption (Optional: for multi-account organizations)
# iam_role_arn: arn:aws:iam::123456789012:role/AnsibleAutomationCrossAccountRole

# Server-side API filters to avoid downloading unnecessary instances
filters:
  instance-state-name:
    - running
  "tag:ManagedBy":
    - Terraform

# Strict host naming precedence: Use EC2 Name tag first, fallback to private DNS
hostnames:
  - "tag:Name"
  - private-dns-name
  - ip-address

# keyed_groups: Automatically generate dynamic Ansible groups based on EC2 attributes
keyed_groups:
  # Group by Environment tag: creates groups 'env_production', 'env_staging'
  - key: tags.Environment
    prefix: env
    separator: "_"

  # Group by Role tag: creates groups 'role_api', 'role_worker'
  - key: tags.Role
    prefix: role
    separator: "_"

  # Group by Availability Zone: creates 'az_us_east_1a', 'az_us_east_1b'
  - key: placement.availability_zone
    prefix: az
    separator: "_"

  # Group by Instance Type: creates 'type_c6i_large'
  - key: instance_type
    prefix: type
    separator: "_"

# compose: Dynamically compute or assign behavioral Ansible variables
compose:
  # In private subnets, always connect to the private IP address!
  ansible_host: private_ip_address
  
  # Inject custom variables based on instance tags
  deployment_tier: tags.Tier | default('unknown')

# Static groups with conditional membership
groups:
  # Add instances to 'monitoring_targets' if they have public monitoring enabled
  monitoring_targets: "tags.Monitoring | default('false') == 'true'"
```

---

## 3. High-Scale API Performance & Caching

In large AWS environments with thousands of EC2 instances, executing `DescribeInstances` on every task or playbook run triggers AWS API throttling:

```text
botocore.exceptions.ClientError: An error occurred (RequestLimitExceeded) 
when calling the DescribeInstances operation: Request limit exceeded.
```

### Implementing Client-Side Caching:
Ansible allows caching the dynamic inventory in local JSON files or Redis:

```ini
# Enable caching in ansible.cfg
[defaults]
inventory = ./inventories/production/production_aws_ec2.yml

# Inventory plugin cache configuration
[inventory]
cache = True
cache_plugin = jsonfile
cache_timeout = 3600  # Cache instances for 1 hour
cache_connection = /tmp/ansible_inventory_cache
```

To manually invalidate the cache and force an immediate refresh from the AWS API:
```bash
ansible-inventory -i production_aws_ec2.yml --refresh-cache --graph
```

---

## 4. Validating and Querying the Dynamic Fleet via CLI

```bash
# 1. Graphically inspect dynamically generated groups and host assignments
ansible-inventory -i production_aws_ec2.yml --graph

# Sample CLI Output:
# @all:
#   |--@az_us_east_1a:
#   |  |--prod-api-01
#   |  |--prod-db-primary
#   |--@env_production:
#   |  |--prod-api-01
#   |  |--prod-api-02
#   |--@role_api:
#   |  |--prod-api-01
#   |  |--prod-api-02

# 2. Inspect all raw attributes extracted from AWS for a single instance
ansible-inventory -i production_aws_ec2.yml --host prod-api-01

# 3. Execute an ad-hoc ping against a dynamically computed group
ansible 'env_production:&role_api' -i production_aws_ec2.yml -m ping
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Dynamic Inventory Plugins vs Legacy Scripts](./04-Dynamic-Inventory-Plugins-vs-Legacy-Scripts.md) | [Index](../../../README.md) | [06 - Constructed Inventory and Multiple Inventory Sources →](./06-Constructed-Inventory-and-Multiple-Inventory-Sources.md) |

