# 05 - Azure DevOps: Service Connections, Workload Identity & Secret Governance

> **Focus:** Secure authentication and credential management in Azure Pipelines. Deep dive into Service Connections, Workload Identity Federation (passwordless OIDC), Variable Groups integrated with Azure Key Vault, Environment approvals, and mitigating secret leakage in CI/CD.

---

## 🔐 1. What, Why, and History of Service Connections

### What is a Service Connection?
A Service Connection is a secure, role-restricted configuration in Azure DevOps that allows pipelines to authenticate and deploy resources into external systems (such as Azure subscriptions, AWS accounts, GitHub, Docker Hub, SonarQube, or Kubernetes clusters) without embedding credentials inside the pipeline YAML file.

### Why did Static Secrets in Pipelines Become a Liability?
1. **Secret Expiration Outages:** Traditional App Registration client secrets expire after 1–2 years. When they expire unannounced, every CI/CD deployment across the company halts instantly.
2. **Credential Theft:** Storing static secrets in pipeline variables creates the risk that malicious PR authors can echo secrets into logs or exfiltrate them.
3. **Audit Complexity:** Security compliance frameworks demand zero static secrets and verifiable short-lived token lifetimes.

### 📜 History & Evolution:
- **2015–2022:** Service Connections required creating a Service Principal with a 1-year symmetric client secret or uploading a management certificate.
- **2023–Present:** Microsoft introduces **Workload Identity Federation (WIF)** based on OpenID Connect (OIDC). Azure Pipelines can now authenticate passwordlessly into Azure Entra ID using ephemeral, cryptographically signed tokens that expire after a few minutes!

---

## ⚡ 2. Workload Identity Federation (OIDC) Passwordless Architecture

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        WORKLOAD IDENTITY FEDERATION (OIDC) FLOW                        │
│                                                                                        │
│   AZURE PIPELINE RUN             AZURE DEVOPS OIDC ISSUER       MICROSOFT ENTRA ID     │
│   ┌────────────────────┐         ┌───────────────────────┐      ┌────────────────────┐ │
│   │ Running Pipeline   │         │ Token Service         │      │ Entra ID Tenant    │ │
│   │ Job on Agent       │         │ (vstoken.dev.azure)   │      │ (contoso.com)      │ │
│   └─────────┬──────────┘         └───────────┬───────────┘      └─────────┬──────────┘ │
│             │                                │                            │            │
│             │ 1. Requests OIDC Token         │                            │            │
│             ├───────────────────────────────>│                            │            │
│             │ 2. Issues signed JWT Token     │                            │            │
│             │<───────────────────────────────┤                            │            │
│             │                                                             │            │
│             │ 3. Presents JWT token directly to Entra ID                  │            │
│             ├────────────────────────────────────────────────────────────>│            │
│             │    (Subject: sc://contoso/Payments-Project/sc-azure-prod)   │            │
│             │                                                             │ 4. Verifies│
│             │                                                             │    Issuer  │
│             │ 5. Returns short-lived OAuth 2.0 Access Token               │    & claims│
│             │<────────────────────────────────────────────────────────────┤            │
│             │                                                                          │
│             ▼                                                                          │
│   Executes ARM REST APIs against Azure subscription with assigned RBAC Contributor role!│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

> **Key Advantage:** ZERO passwords, ZERO secrets, ZERO rotation schedules, and mathematically impossible to leak an expired master key!

---

## 📦 3. Linking Variable Groups Directly to Azure Key Vault

Instead of copying sensitive database connection strings or third-party API keys into Azure DevOps, link a Variable Group directly to an Azure Key Vault:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              KEY VAULT VARIABLE GROUP FLOW                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. Secrets reside strictly inside Azure Key Vault (vault-corp-secrets).                │
│ 2. Azure DevOps Variable Group 'vg-prod-secrets' is configured with toggle:             │
│    "Link secrets from an Azure key vault as variables".                                │
│ 3. When pipeline executes, the agent fetches secrets directly from Key Vault in-memory.│
│ 4. Azure DevOps automatically masks all secret values in build logs (rendered as ***). │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Consuming Service Connections & Secrets in YAML

```yaml
# Pipeline consuming Workload Identity Service Connection and Key Vault secrets
trigger: none

variables:
  - group: vg-prod-secrets # Key Vault linked variable group (provides $(DatabasePassword))

jobs:
  - job: DeployDatabaseMigration
    pool:
      vmImage: 'ubuntu-latest'
    steps:
      - task: AzureCLI@2
        displayName: 'Execute Azure CLI via Passwordless Service Connection'
        inputs:
          azureSubscription: 'sc-azure-workload-identity' # Configured with OIDC WIF!
          scriptType: 'bash'
          scriptLocation: 'inlineScript'
          inlineScript: |
            echo "Successfully authenticated to Azure as $(az account show --query user.name -o tsv)"
            echo "Deploying database changes to resource group rg-databases..."
            az sql db list --resource-group rg-databases --server sql-banking-prod -o table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Agent Pools Microsoft Hosted vs Self Hosted Agents](./04-Agent-Pools-Microsoft-Hosted-vs-Self-Hosted-Agents.md) | [Index](../../../README.md) | [06 - Azure Artifacts and Package Management →](./06-Azure-Artifacts-and-Package-Management.md) |
