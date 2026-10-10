# 05 - Terraform Graph, DAG & Parallelism

## 1. What Is the Directed Acyclic Graph (DAG) in Terraform?

At the very core of Terraform's declarative engine lies a mathematical computer science structure called the **Directed Acyclic Graph (DAG)**.

Unlike imperative scripting languages (such as Bash or Python) where execution flows strictly from line 1 to line 100 in sequential order, **Terraform evaluates configuration files non-sequentially**. You can define an EC2 instance on line 1 and the VPC it belongs to on line 500 across a completely different `.tf` file. 

Terraform does not care about the order of lines or files. Instead, Terraform Core parses all configuration files in the directory, identifies every resource, data source, provider, and variable, and constructs a dependency graph:
1. **Directed:** The connections (edges) between resources have a strict, one-way direction indicating dependency ($B \to A$, meaning Resource B requires Resource A to exist first).
2. **Acyclic:** The graph contains **zero closed loops or circular cycles**. If Resource A depends on B, B depends on C, and C depends on A, the graph is invalid and Terraform refuses to execute.
3. **Graph:** A topological mathematical model that enables Terraform to calculate the exact optimal execution sequence and provision independent resources concurrently.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE DIRECTED ACYCLIC GRAPH (DAG) TOPOLOGY                                  |
+---------------------------------------------------------------------------------------------------+

                               ┌───────────────────────────┐
                               │       Root Node: VPC      │
                               │      (aws_vpc.main)       │
                               └─────────────┬─────────────┘
                                             │ (Parent Dependency)
         ┌───────────────────────────────────┼───────────────────────────────────┐
         ▼                                   ▼                                   ▼
 ┌──────────────┐                    ┌──────────────┐                    ┌──────────────┐
 │ Subnet A     │                    │ Subnet B     │                    │ Internet GW  │
 │ (Zone 1a)    │                    │ (Zone 1b)    │                    │ (aws_igw)    │
 └──────┬───────┘                    └──────┬───────┘                    └──────┬───────┘
        │                                   │                                   │
        └─────────────────┬─────────────────┘                                   │
                          ▼                                                     ▼
        ┌───────────────────────────────────┐                    ┌──────────────────────────────┐
        │ Database Subnet Group             │                    │ Public Routing Table Entry   │
        │ (aws_db_subnet_group.default)     │                    │ (aws_route.public_internet)  │
        └─────────────────┬─────────────────┘                    └──────────────────────────────┘
                          ▼
        ┌───────────────────────────────────┐
        │ Managed RDS PostgreSQL Cluster   │
        │ (aws_db_instance.postgres)        │
        └───────────────────────────────────┘

 PARALLEL EXECUTION GUARANTEE:
 Subnet A, Subnet B, and Internet Gateway have NO dependencies on each other.
 Terraform creates all three SIMULTANEOUSLY in parallel!
```

---

## 2. Implicit vs. Explicit Dependencies

Terraform distinguishes between dependencies inferred automatically from code and dependencies declared manually by the engineer:

### 1. Implicit Dependencies (The Best Practice Standard)
Terraform automatically infers dependencies whenever an attribute of one resource directly references an exported attribute of another resource:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "public" {
  # Implicit dependency created automatically via reference:
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}
```
Terraform's HCL parser analyzes the expression `aws_vpc.main.id`. It automatically draws a directed edge in the graph: `aws_subnet.public ──► aws_vpc.main`. You never need to instruct Terraform to build the VPC first; the graph enforces it inherently.

### 2. Explicit Dependencies (`depends_on`)
Occasionally, two resources have a strict hidden operational dependency that is not visible through attribute references. For example, an AWS Lambda function requires an IAM Role Policy attachment to be fully active in AWS IAM before the Lambda code can execute, but the Lambda resource block only references the Role itself, not the Policy Attachment.

In these specific scenarios, declare an explicit dependency using the **`depends_on`** meta-argument:

```hcl
resource "aws_iam_role_policy_attachment" "lambda_logs" {
  role       = aws_iam_role.iam_for_lambda.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
}

resource "aws_lambda_function" "processor" {
  function_name = "order-processor"
  role          = aws_iam_role.iam_for_lambda.arn
  # Explicit dependency guarantees policy attachment completes first:
  depends_on = [
    aws_iam_role_policy_attachment.lambda_logs
  ]
}
```

> [!WARNING]
> **Avoid overusing `depends_on`!** Declaring unnecessary `depends_on` blocks artificially flattens your dependency graph into a slow, sequential pipeline, completely disabling Terraform's ability to provision resources in parallel.

---

## 3. The Dreaded "Cycle" Error and How to Break It

A **Circular Dependency (Cycle)** occurs when Resource A depends on Resource B, while Resource B simultaneously depends on Resource A.

A classic real-world example involves AWS Security Groups:
- Security Group `app_sg` wants to allow inbound traffic only from `db_sg`.
- Security Group `db_sg` wants to allow inbound traffic only from `app_sg`.

If declared inline within the security group resources, Terraform halts during graph compilation:
```text
Error: Cycle: aws_security_group.app_sg, aws_security_group.db_sg
```

### The Architectural Solution: Decouple via Standalone Resources
To break the cycle, declare the security groups as empty containers first, and define the cross-referencing rules as independent, standalone `aws_security_group_rule` resources:

```hcl
# 1. Base security groups have zero mutual dependencies:
resource "aws_security_group" "app_sg" {
  name   = "app-security-group"
  vpc_id = aws_vpc.main.id
}

resource "aws_security_group" "db_sg" {
  name   = "db-security-group"
  vpc_id = aws_vpc.main.id
}

# 2. Standalone rule connects app to db cleanly:
resource "aws_security_group_rule" "app_to_db" {
  type                     = "ingress"
  from_port                = 5432
  to_port                  = 5432
  protocol                 = "tcp"
  security_group_id        = aws_security_group.db_sg.id
  source_security_group_id = aws_security_group.app_sg.id
}
```

---

## 4. Concurrency & Parallelism Tuning (`-parallelism=N`)

During the graph walk, Terraform walks all independent branches concurrently using a worker pool of Go goroutines. The maximum number of simultaneous concurrent operations is controlled by the **`-parallelism`** flag (default: **10**).

```bash
# Default concurrency: 10 simultaneous resource operations
terraform apply

# High-velocity deployment for large enterprise estates (500+ resources):
terraform apply -parallelism=30

# Throttled deployment to prevent cloud API rate limits:
terraform apply -parallelism=3
```

### The Cloud API Throttling Trade-Off:
Increasing parallelism speeds up deployments, but aggressive concurrency can trigger **HTTP 429 Too Many Requests / RequestLimitExceeded** errors from cloud APIs (e.g., AWS EC2 or Cloudflare rate limits). In accounts with strict API quotas, reducing `-parallelism` to `3` or `5` prevents transient deployment failures.

---

## 5. Visualizing the Dependency Graph

You can export Terraform's internal DAG directly to standard DOT format and render it as an image using Graphviz:

```bash
# Export the execution graph to Graphviz DOT format
terraform graph > graph.dot

# Render to a high-resolution PNG image (requires graphviz installed)
dot -Tpng graph.dot -o terraform-architecture-graph.png

# Highlight cycle errors if compilation fails
terraform graph -draw-cycles
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Backend Configuration Local S3 GCS AzureRM](./04-Backend-Configuration-Local-S3-GCS-AzureRM.md) | [Index](../../../README.md) | [06 - Version Management tfenv tofuenv and Required Versions →](./06-Version-Management-tfenv-tofuenv-and-Required-Versions.md) |
