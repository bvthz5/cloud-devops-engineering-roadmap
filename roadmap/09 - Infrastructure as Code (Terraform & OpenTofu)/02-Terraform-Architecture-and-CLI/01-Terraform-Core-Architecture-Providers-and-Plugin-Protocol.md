# 01 - Terraform Core Architecture, Providers & Plugin Protocol

## 1. High-Level Architecture

```text
┌────────────────────────────────────────────────────────────────┐
│                    TERRAFORM / OPENTOFU CORE                    │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    CORE BINARY                            │  │
│  │  ┌─────────────┐ ┌─────────────┐ ┌───────────────────┐  │  │
│  │  │   HCL       │ │  DAG Engine │ │  State Manager    │  │  │
│  │  │   Parser    │ │  (Graph)    │ │  (Read/Write)     │  │  │
│  │  └──────┬──────┘ └──────┬──────┘ └────────┬──────────┘  │  │
│  │         │               │                  │              │  │
│  │         └───────────────┼──────────────────┘              │  │
│  │                         │                                 │  │
│  │                    ┌────┴─────┐                           │  │
│  │                    │ gRPC Hub │                           │  │
│  │                    └────┬─────┘                           │  │
│  └─────────────────────────┼────────────────────────────────┘  │
│                            │ gRPC Protocol v5/v6               │
│  ┌─────────────────────────┼────────────────────────────────┐  │
│  │                    PROVIDER PLUGINS                       │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │  │
│  │  │   AWS    │  │  Azure   │  │   GCP    │  │  K8s    │ │  │
│  │  │ Provider │  │ Provider │  │ Provider │  │Provider │ │  │
│  │  └──────────┘  └──────────┘  └──────────┘  └─────────┘ │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────┘
```

## 2. Core Components

### 2.1 HCL Parser
- Parses `.tf` files written in HashiCorp Configuration Language (HCL)
- Converts human-readable config into an Abstract Syntax Tree (AST)
- Supports JSON alternative syntax (`.tf.json`)

### 2.2 DAG Engine (Directed Acyclic Graph)
- Builds a dependency graph of all resources
- Determines execution order based on implicit and explicit dependencies
- Enables parallel resource creation where dependencies allow

### 2.3 State Manager
- Reads current state from backend (local file or remote storage)
- Compares desired state (HCL) with actual state (state file + cloud API)
- Writes updated state after successful operations

### 2.4 Provider Plugin Protocol (gRPC v5/v6)
- Providers are **separate binaries** communicating via gRPC
- Each provider implements CRUD operations for its resource types
- Plugin protocol versions: v5 (Terraform ≤ 1.x), v6 (modern providers)
- Providers are downloaded during `terraform init`

## 3. The `terraform init` Lifecycle

```text
terraform init
    │
    ├── 1. Parse backend configuration
    │      └── Initialize backend (S3, GCS, local, etc.)
    │
    ├── 2. Download required providers
    │      └── From registry.terraform.io or configured mirror
    │      └── Store in .terraform/providers/
    │
    ├── 3. Generate dependency lock file
    │      └── .terraform.lock.hcl (commit to Git!)
    │
    └── 4. Initialize modules
           └── Download referenced modules to .terraform/modules/
```

## 4. Provider Source Addressing

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"      # <NAMESPACE>/<TYPE>
      version = "~> 5.0"             # Pessimistic constraint
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = ">= 2.20, < 3.0"    # Range constraint
    }
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Provider Registry](./02-Provider-Registry-Installation-and-Version-Pinning.md) |
