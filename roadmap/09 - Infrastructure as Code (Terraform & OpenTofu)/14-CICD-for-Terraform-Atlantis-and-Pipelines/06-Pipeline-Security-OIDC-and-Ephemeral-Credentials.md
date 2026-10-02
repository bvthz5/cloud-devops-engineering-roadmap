# 06 - Pipeline Security: OIDC & Ephemeral Credentials

## 1. The Problem with Static Keys

Long-lived AWS access keys in CI secrets are a major security risk (leaked keys = full account compromise).

## 2. OIDC Federation (Keyless Auth)

```text
GitHub Actions --> OIDC token --> AWS STS AssumeRoleWithWebIdentity
                                       |
                                       v
                                  Short-lived credentials (15 min - 1 hour)
```

## 3. AWS OIDC Setup

```hcl
resource "aws_iam_openid_connect_provider" "github" {
  url             = "https://token.actions.githubusercontent.com"
  client_id_list  = ["sts.amazonaws.com"]
  thumbprint_list = ["6938fd4d98bab03faadb97b34396831e3780aea1"]
}

resource "aws_iam_role" "github_actions" {
  name = "GitHubActionsRole"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = { Federated = aws_iam_openid_connect_provider.github.arn }
      Action = "sts:AssumeRoleWithWebIdentity"
      Condition = {
        StringEquals = { "token.actions.githubusercontent.com:aud" = "sts.amazonaws.com" }
        StringLike   = { "token.actions.githubusercontent.com:sub" = "repo:myorg/infra:*" }
      }
    }]
  })
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Jenkins Pipeline](./05-Jenkins-Terraform-Pipeline.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
