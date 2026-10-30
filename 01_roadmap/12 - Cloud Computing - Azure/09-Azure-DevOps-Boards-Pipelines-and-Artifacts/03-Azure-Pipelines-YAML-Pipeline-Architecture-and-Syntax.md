# 03 - Azure Pipelines: Multi-Stage YAML Pipeline Architecture & Syntax

> **Focus:** Modern Pipeline-as-Code in Azure DevOps. Deep dive into Multi-Stage YAML architecture (`stages` ➔ `jobs` ➔ `steps` ➔ `tasks`), CI triggers, deployment jobs, environment gates, variable groups, and reusable YAML templates.

---

## ⚡ 1. What, Why, and History of Azure Pipelines YAML

### What is Azure Pipelines YAML?
Azure Pipelines YAML is a declarative, version-controlled **Pipeline-as-Code** framework that defines the complete build, test, and deployment lifecycle of an application inside a text file (`azure-pipelines.yml`) committed directly into your Git repository.

### Why did YAML Replace Classic UI Pipelines?
1. **Version Controlled with Application Code:** When a developer refactors code to require a new SDK version or build step in a Git branch, the pipeline configuration branches alongside the code. Merging the branch updates the build automatically.
2. **Pull Request Validation:** Changes to the CI/CD pipeline itself undergo peer review, linting, and approval in Pull Requests just like application source code.
3. **Template Reusability:** Security teams can author centralized, immutable security-scanning YAML templates that every microservice across 500 repositories must include.

### 📜 History & Evolution:
- **2015:** Visual Studio Team Services introduces **Classic Build & Release Pipelines** operated via drag-and-drop web UI forms.
- **2018:** Microsoft introduces single-stage YAML for CI builds.
- **2020:** **Multi-Stage YAML Pipelines** officially achieve GA, combining Build (CI) and Release (CD) into a single unified file and deprecating Classic UI releases for new projects.

---

## 🏗️ 2. The 4-Tier YAML Pipeline Structural Hierarchy

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE PIPELINES YAML HIERARCHY                            │
│                                                                                        │
│   PIPELINE (The Entire Workflow: azure-pipelines.yml)                                  │
│   ├── Triggers (branches, tags, PR paths)                                              │
│   ├── Variables & Variable Groups                                                      │
│   │                                                                                    │
│   └── STAGES (Major Logical Phases, e.g., Build, Staging, Production)                  │
│       │                                                                                │
│       └── JOBS (Units of Execution running on an assigned Agent)                       │
│           │ • Regular Job (Compiles code, runs tests in parallel)                      │
│           │ • Deployment Job (Deploys to an 'Environment' with approvals & rollbacks)  │
│           │                                                                            │
│           └── STEPS (Sequential operations executed inside the assigned container/VM) │
│               ├── Task: Built-in plugin (e.g., Docker@2, AzureResourceManagerTemplate) │
│               ├── Script / Bash: Custom shell commands (bash, pwsh, cmd)               │
│               └── Checkout: Clones Git repository onto agent disk                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📜 3. Complete Production Multi-Stage YAML Pipeline Template

```yaml
name: $(Date:yyyyMMdd)$(Rev:.r)-production-release

# 1. Pipeline Triggers
trigger:
  branches:
    include:
      - main
  paths:
    exclude:
      - README.md
      - docs/*

# 2. Global Variables & Variable Groups
variables:
  - group: vg-production-secrets
  - name: imageRepository
    value: 'payments-api'
  - name: dockerfilePath
    value: '$(Build.SourcesDirectory)/Dockerfile'
  - name: tag
    value: '$(Build.BuildId)'

# 3. Multi-Stage Pipeline Execution
stages:
  # ==========================================
  # STAGE 1: COMPILE, TEST & DOCKER BUILD
  # ==========================================
  - stage: BuildAndTest
    displayName: 'Build, Unit Test & Package'
    pool:
      vmImage: 'ubuntu-latest'
    jobs:
      - job: BuildJob
        displayName: 'Compile & Run Tests'
        steps:
          - checkout: self

          - task: NodeTool@0
            inputs:
              versionSpec: '20.x'
            displayName: 'Install Node.js 20'

          - script: |
              npm ci
              npm run test:coverage
            displayName: 'Execute Automated Unit Tests'

          - task: Docker@2
            displayName: 'Build and Push Docker Image to ACR'
            inputs:
              command: buildAndPush
              repository: $(imageRepository)
              dockerfile: $(dockerfilePath)
              containerRegistry: 'sc-acr-production'
              tags: |
                $(tag)
                latest

  # ==========================================
  # STAGE 2: PRODUCTION DEPLOYMENT
  # ==========================================
  - stage: DeployProduction
    displayName: 'Deploy to Production AKS'
    dependsOn: BuildAndTest
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
    jobs:
      - deployment: DeployProdJob
        displayName: 'Deploy Microservice to Kubernetes'
        pool:
          vmImage: 'ubuntu-latest'
        environment: 'production-banking-cluster' # Protected by Manual Approvals in UI!
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureKubernetesService@1
                  displayName: 'Apply K8s Manifests'
                  inputs:
                    connectionType: 'Azure Resource Manager'
                    azureSubscriptionEndpoint: 'sc-azure-production'
                    azureResourceGroup: 'rg-aks-prod'
                    kubernetesCluster: 'aks-cluster-prod'
                    command: 'set'
                    arguments: 'image deployment/payments-deploy payments=$(imageRepository):$(tag)'
```

---

## 🛡️ 4. Environments & Approvals: The Production Gate

In Azure Pipelines, declaring an `environment: 'production-banking-cluster'` transforms a normal job into a **Deployment Job**:
- **Manual Approvals:** Azure DevOps pauses execution and notifies designated release managers via email/Teams to review and sign off before code hits production.
- **Deployment History & Auditability:** Tracks exactly which Git commit is currently running on each target cluster or VM.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure Repos and Branch Policies](./02-Azure-Repos-and-Branch-Policies.md) | [Index](../../../README.md) | [04 - Agent Pools Microsoft Hosted vs Self Hosted Agents →](./04-Agent-Pools-Microsoft-Hosted-vs-Self-Hosted-Agents.md) |
