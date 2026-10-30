# 03 - Pulumi Architecture & State Management

Pulumi represents a paradigm shift in Infrastructure as Code by replacing domain-specific configuration languages (such as HCL) with general-purpose programming languages (TypeScript/JavaScript, Python, Go, C#/.NET, and Java). Rather than interpreting static configuration files, Pulumi executes standard compiled or interpreted code that communicates with a centralized orchestrator over high-speed RPC channels.

---

## 1. Deep Architecture: Language Host, Engine, and Providers

Pulumi decouples code execution from infrastructure reconciliation using a modular, three-tier architecture:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          PULUMI SYSTEM ARCHITECTURE                             │
│                                                                                 │
│   TIER 1: LANGUAGE HOST (Runtime)                                               │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │ Node.js / Python / Go / .NET Execution Process                          │   │
│   │ Executes index.ts or __main__.py                                        │   │
│   │ Evaluates loops, functions, classes, and constructs Output<T> promises  │   │
│   └────────────────────────────────────┬────────────────────────────────────┘   │
│                                        │ gRPC Resource Registrations            │
│                                        ▼                                        │
│   TIER 2: PULUMI ENGINE (Core Orchestrator)                                     │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │ • Computes Directed Acyclic Graph (DAG) dependencies                    │   │
│   │ • Calculates State Delta (Desired State vs. Checkpoint Snapshot)        │   │
│   │ • Evaluates Policy as Code (CrossGuard)                                 │   │
│   │ • Coordinates concurrency locking                                       │   │
│   └────────────────────────────────────┬────────────────────────────────────┘   │
│                                        │ gRPC Provider Plugin Protocol          │
│                                        ▼                                        │
│   TIER 3: RESOURCE PROVIDERS (Cloud Integration)                                │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │ Native Providers: AWS Native, Azure Native (Direct OpenAPI mapping)     │   │
│   │ Bridged Providers: pulumi-aws (Wraps HashiCorp Terraform AWS Provider)  │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### How the Bridge Works (`pulumi-terraform-bridge`)
Many of Pulumi's most popular providers (such as `pulumi-aws` and `pulumi-azure`) leverage the **Pulumi Terraform Bridge**. This bridge wraps official Terraform Provider binaries in a translation layer, allowing Pulumi to instantly inherit support for thousands of AWS resources while exposing clean, idiomatic TypeScript and Python SDKs.

---

## 2. The Core Execution Model: Understanding `Output<T>` and Promises

Because cloud resources have attributes that only exist *after* provisioning (such as an EC2 instance's generated Public IP or an RDS Aurora endpoint), Pulumi treats all resource attributes as asynchronous futures wrapped in the **`Output<T>`** type.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       THE OUTPUT<T> PROMISE PIPELINE                            │
│                                                                                 │
│   1. Declare VPC:                  2. Declare Subnet:                           │
│   const vpc = new aws.ec2.Vpc()    const subnet = new aws.ec2.Subnet({          │
│   vpc.id -> Output<string>                 vpcId: vpc.id                        │
│   (Value UNKNOWN during compile)   })                                           │
│                                            │                                    │
│   3. Engine intercepts dependency:         ▼                                    │
│      Resolves `vpc.id` only AFTER VPC creation API returns from AWS!            │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Production Example (TypeScript)

```typescript
import * as pulumi from "@pulumi/pulumi";
import * as aws from "@pulumi/aws";

// 1. Create a VPC
const vpc = new aws.ec2.Vpc("production-vpc", {
    cidrBlock: "10.0.0.0/16",
    enableDnsHostnames: true,
    tags: { Environment: "production" },
});

// 2. Create an Internet Gateway
const gateway = new aws.ec2.InternetGateway("gw", {
    vpcId: vpc.id, // vpc.id is an Output<string>; engine handles dependency automatically
});

// 3. Transform outputs dynamically using .apply()
const connectionString = pulumi.all([vpc.id, gateway.id]).apply(([vpcId, gwId]) => {
    return `VPC ${vpcId} successfully bridged to Gateway ${gwId}`;
});

// Export attributes to stack output
export const vpcId = vpc.id;
export const statusMessage = connectionString;
```

---

## 3. Pulumi State & Secrets Management

Like Terraform, Pulumi requires a state file (known as a **checkpoint**) to record the mapping between code resources and real-world cloud URNs.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      PULUMI STATE BACKEND ARCHITECTURE                          │
│                                                                                 │
│   MANAGED BACKEND (Default)                  SELF-HOSTED BACKEND                │
│   ┌───────────────────────────┐              ┌──────────────────────────────┐   │
│   │ Pulumi Cloud (SaaS)       │              │ AWS S3 / GCS / Azure Blob    │   │
│   │ • Built-in state locking  │              │ pulumi login s3://my-bucket  │   │
│   │ • Web dashboard UI        │              │ • Manual locking via bucket  │   │
│   │ • Automatic team RBAC     │              │ • Free, self-contained      │   │
│   │ • Secret manager integration             │ • Requires custom encryption │   │
│   └───────────────────────────┘              └──────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Native Value-Level Secret Encryption
Unlike open-source Terraform which historically stores secrets in plaintext inside `terraform.tfstate`, Pulumi encrypts secrets **at the individual value level** before writing to the state file:

```typescript
// Marking a database password as an encrypted secret
const dbPassword = new pulumi.Config().requireSecret("dbPassword");

const db = new aws.rds.Instance("app-db", {
    engine: "postgres",
    instanceClass: "db.t3.medium",
    password: dbPassword, // Encrypted with AES-GCM or KMS key inside state!
});
```

When inspect the raw state checkpoint JSON, the password attribute is rendered as:
```json
{
  "password": {
    "4dabf1819302": "1b4706124f21",
    "ciphertext": "v1:G8+oXb9...[ENCRYPTED BLOB]..."
  }
}
```

---

## 4. Comprehensive Comparison: Pulumi vs. Terraform State

| Architectural Feature | HashiCorp Terraform | Pulumi |
| :--- | :--- | :--- |
| **Language Paradigm** | Domain-Specific Language (HCL2) | General-Purpose Languages (TypeScript, Python, Go) |
| **State File Format** | JSON (`terraform.tfstate`) | JSON checkpoint snapshot |
| **Default Backend** | Local disk (`terraform.tfstate`) | Managed SaaS (**Pulumi Cloud**) |
| **Alternative Backends** | Amazon S3, Google Cloud Storage, Azure Blob | Amazon S3, Google Cloud Storage, Azure Blob, Local |
| **State Locking** | Amazon DynamoDB / GCS Object Locks | Native SaaS locking or cloud object locks |
| **Secrets in State** | Plaintext by default (Requires OpenTofu for native state encryption) | **Encrypted at the field level** using KMS / Passphrase |
| **Unit Testing** | `terraform test` (HCL based) | Standard frameworks (**Jest, Mocha, PyTest, Go test**) |

---

## 5. Summary & Best Practices

1. **Use `pulumi.all()` for Multiple Outputs:** When combining outputs from two different resources, avoid nested `.apply()` blocks. Use `pulumi.all([res1.id, res2.id]).apply(([id1, id2]) => ...)` to keep asynchronous code clean.
2. **Never Commit Secrets:** Always set secrets using `pulumi config set --secret myKey myValue` so that secrets are encrypted with the stack encryption provider.
3. **Configure Self-Managed Backends Cleanly:** For air-gapped or strictly self-hosted enterprises, run `pulumi login --cloud-url s3://corp-pulumi-state?region=us-east-1` with server-side KMS encryption enabled.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - CDKTF Constructs and Stacks](./02-CDKTF-Constructs-and-Stacks.md) | [Index](../../../README.md) | [04 - HCL vs General Purpose Language Tradeoffs →](./04-HCL-vs-General-Purpose-Language-Tradeoffs.md) |

