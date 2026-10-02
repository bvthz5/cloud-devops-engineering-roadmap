# 04 - Private Registry & Run Tasks

## 1. Private Module Registry

Publish internal modules visible only to your organization.

```text
Organization: acme-corp
Private Registry:
  +-- acme-corp/vpc/aws (v2.1.0)
  +-- acme-corp/eks/aws (v1.5.0)
  +-- acme-corp/rds/aws (v3.0.0)
```

## 2. Run Tasks

Run Tasks are webhooks triggered before or after a plan, enabling integrations with external tools.

| Run Task | Phase | Purpose |
|---|---|---|
| Infracost | Pre-plan | Cost estimation in PR comments |
| Snyk / tfsec | Post-plan | Security scanning |
| ServiceNow | Pre-apply | Change management ticket validation |
| Custom webhook | Any | Custom validation logic |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Sentinel Policies](./03-Sentinel-Policy-Enforcement.md) | [README](./README.md) | [05 - Agent Pools](./05-Agent-Pools-and-Self-Hosted-Runners.md) |
