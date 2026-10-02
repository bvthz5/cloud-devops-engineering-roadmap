# 03 - Azure Pipelines YAML Architecture

```yaml
trigger:
  - main

stages:
  - stage: Build
    jobs:
      - job: BuildJob
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - script: echo Building artifact...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure Repos and Branch Policies](./02-Azure-Repos-and-Branch-Policies.md) | [Index](../../../README.md) | [04 - Agent Pools Microsoft Hosted vs Self Hosted Agents →](./04-Agent-Pools-Microsoft-Hosted-vs-Self-Hosted-Agents.md) |
