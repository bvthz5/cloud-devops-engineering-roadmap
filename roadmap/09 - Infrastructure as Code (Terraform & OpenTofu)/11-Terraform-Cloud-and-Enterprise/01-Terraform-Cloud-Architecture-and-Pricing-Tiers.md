# 01 - Terraform Cloud Architecture & Pricing Tiers

## 1. Architecture Overview

```text
+-------------------------------------------------------------------+
|                    TERRAFORM CLOUD (HCP)                           |
|                                                                     |
|  +------------------+    +-----------------+    +----------------+ |
|  |  VCS Integration |    |   Run Queue     |    |  State Store   | |
|  |  GitHub/GitLab   |--->|  Plan + Apply   |--->|  Encrypted     | |
|  +------------------+    +-----------------+    +----------------+ |
|           |                      |                      |          |
|  +------------------+    +-----------------+    +----------------+ |
|  | Policy Engine    |    |  Cost Estimate  |    |  Private       | |
|  | Sentinel / OPA   |    |  (Infracost)    |    |  Registry      | |
|  +------------------+    +-----------------+    +----------------+ |
+-------------------------------------------------------------------+
```

## 2. Pricing Tiers

| Feature | Free | Team ($20/user/mo) | Business | Enterprise |
|---|---|---|---|---|
| Workspaces | Up to 500 | Unlimited | Unlimited | Unlimited |
| State management | Yes | Yes | Yes | Yes |
| Remote runs | Yes | Yes | Yes | Yes |
| Team management | No | Yes | Yes | Yes |
| Sentinel policies | No | No | Yes | Yes |
| SSO / SAML | No | No | Yes | Yes |
| Audit logging | No | No | Yes | Yes |
| Self-hosted agents | No | No | Yes | Yes |
| Private installs | No | No | No | Yes (TFE) |

## 3. Terraform Cloud vs Terraform Enterprise

| Aspect | Terraform Cloud | Terraform Enterprise |
|---|---|---|
| Hosting | SaaS (HashiCorp-managed) | Self-hosted (your infrastructure) |
| Network | Public internet | Private network (air-gapped capable) |
| Compliance | SOC 2 Type II | Customer-controlled |
| Updates | Automatic | Manual upgrade process |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - VCS-Driven Runs](./02-VCS-Driven-Runs-and-Speculative-Plans.md) |
