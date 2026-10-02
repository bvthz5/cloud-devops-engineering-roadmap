# 01 - Terraform State: Purpose, Structure & Internals

## 1. Why State Exists

Terraform state maps your configuration to real-world resources. Without it, Terraform cannot determine what currently exists, what needs to change, or what to destroy.

```text
┌────────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│   HCL Config       │     │   State File      │     │   Real Cloud      │
│   (Desired State)  │────►│   (Known State)   │────►│   (Actual State)  │
│                    │     │   terraform.tfstate│     │   AWS / GCP / Az  │
└────────────────────┘     └───────────────────┘     └───────────────────┘
         │                          │                          │
         └──── terraform plan ──────┘──── API queries ────────┘
                  (computes diff)
```

## 2. State File JSON Structure

```json
{
  "version": 4,
  "terraform_version": "1.7.0",
  "serial": 42,
  "lineage": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "outputs": {
    "vpc_id": {
      "value": "vpc-0abc123def456",
      "type": "string"
    }
  },
  "resources": [
    {
      "mode": "managed",
      "type": "aws_vpc",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "attributes": {
            "id": "vpc-0abc123def456",
            "cidr_block": "10.0.0.0/16",
            "tags": { "Name": "main-vpc" }
          }
        }
      ]
    }
  ]
}
```

## 3. Key State Concepts

| Field | Purpose |
|---|---|
| `version` | State file format version (currently 4) |
| `serial` | Incremented on every state write (optimistic locking) |
| `lineage` | UUID identifying this state's lineage (prevents cross-state contamination) |
| `outputs` | Values exported via `output` blocks |
| `resources` | Array of all managed resources with their cloud-side attributes |

## 4. State Operations Flow

```text
terraform plan
    │
    ├── 1. Read state file (local or remote backend)
    ├── 2. Refresh: Query cloud APIs for actual resource state
    ├── 3. Compare: HCL desired state vs actual state
    └── 4. Generate execution plan (diff)

terraform apply
    │
    ├── 1. Execute planned changes via provider APIs
    └── 2. Write updated state (serial incremented)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Remote Backends](./02-Remote-State-Backends-S3-GCS-AzureRM-Consul.md) |
