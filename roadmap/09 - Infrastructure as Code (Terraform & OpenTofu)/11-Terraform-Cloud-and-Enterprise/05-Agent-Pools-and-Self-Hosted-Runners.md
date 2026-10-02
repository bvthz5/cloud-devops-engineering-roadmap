# 05 - Agent Pools & Self-Hosted Runners

## 1. Why Self-Hosted Agents?

Terraform Cloud runs execute on HashiCorp-managed infrastructure by default. Self-hosted agents are needed when:
- Infrastructure is in a private network (no public internet access)
- Compliance requires runs to execute within your network boundary
- Custom tooling (Ansible, Packer, custom CLIs) is needed during runs

## 2. Agent Architecture

```text
+---------------------------+      +---------------------------+
|   TERRAFORM CLOUD (SaaS)  |      |   YOUR PRIVATE NETWORK    |
|                            |      |                           |
|   Workspace: prod-vpc     |<---->|   tfc-agent (container)   |
|   (uses agent pool)       | TLS  |   tfc-agent (container)   |
|                            |      |   tfc-agent (container)   |
+---------------------------+      +---------------------------+
```

## 3. Agent Deployment

```bash
# Docker-based agent
docker run -d \
  -e TFC_AGENT_TOKEN=<token> \
  -e TFC_AGENT_NAME=agent-01 \
  hashicorp/tfc-agent:latest
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Private Registry and Run Tasks](./04-Private-Registry-and-Run-Tasks.md) | [Index](../../../README.md) | [06 - Cost Estimation and Audit Logging →](./06-Cost-Estimation-and-Audit-Logging.md) |
