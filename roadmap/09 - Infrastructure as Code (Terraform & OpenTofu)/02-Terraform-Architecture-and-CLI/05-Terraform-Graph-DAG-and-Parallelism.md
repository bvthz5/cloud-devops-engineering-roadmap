# 05 - Terraform Graph, DAG & Parallelism

## 1. The Directed Acyclic Graph (DAG)

Terraform builds a **DAG** of all resources to determine the correct order of operations. Resources with no dependencies can be created in parallel.

```text
                    ┌─────────────┐
                    │  aws_vpc    │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       ┌──────────┐ ┌──────────┐ ┌──────────┐
       │ subnet_a │ │ subnet_b │ │ igw      │
       └────┬─────┘ └────┬─────┘ └────┬─────┘
            │             │            │
            └──────┬──────┘            │
                   ▼                   │
            ┌──────────┐              │
            │   alb    │◄─────────────┘
            └────┬─────┘
                 ▼
          ┌──────────┐
          │ ec2_asg  │
          └──────────┘
```

**Parallel execution:** `subnet_a`, `subnet_b`, and `igw` can be created simultaneously because they only depend on `aws_vpc`, not on each other.

## 2. Visualizing the Graph

```bash
# Generate DOT format graph
terraform graph | dot -Tpng > graph.png

# Generate graph for destroy operations
terraform graph -type=destroy | dot -Tsvg > destroy-graph.svg

# Human-readable format
terraform graph -draw-cycles   # Highlight circular dependencies
```

## 3. Parallelism Tuning

```bash
# Default parallelism: 10 concurrent operations
terraform apply -parallelism=10

# Increase for large deployments
terraform apply -parallelism=30

# Decrease if hitting API rate limits
terraform apply -parallelism=2
```

| Scenario | Recommended Parallelism |
|---|---|
| Small projects (< 50 resources) | Default (10) |
| Large deployments (500+ resources) | 20-30 |
| API rate-limited providers | 2-5 |
| Debugging dependency issues | 1 |

## 4. Implicit vs Explicit Dependencies

```hcl
# Implicit dependency (Terraform detects via attribute reference)
resource "aws_subnet" "main" {
  vpc_id = aws_vpc.main.id   # ← Terraform knows subnet depends on VPC
}

# Explicit dependency (when there's no attribute reference)
resource "aws_instance" "app" {
  depends_on = [aws_iam_role_policy.app_policy]  # ← Manual dependency
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Backend Configuration](./04-Backend-Configuration-Local-S3-GCS-AzureRM.md) | [README](./README.md) | [06 - Version Management](./06-Version-Management-tfenv-tofuenv-and-Required-Versions.md) |
