# 01 - Azure DevOps: Organization, Project Architecture & SDLC Governance

> **Focus:** Enterprise structure of Azure DevOps (ADO). Deep dive into Organizations, Projects, Process Templates (Agile, Scrum, Basic, CMMI), Microsoft Entra ID tenant binding, Service boundaries, and cross-project governance.

---

## 🏛️ 1. What, Why, and History of Azure DevOps

### What is Azure DevOps?
Azure DevOps is Microsoft's enterprise Software Development Lifecycle (SDLC) platform, providing an integrated suite of five core cloud services:
1. **Azure Boards:** Agile planning, Kanban boards, sprint backlogs, and issue tracking.
2. **Azure Repos:** Private Git repositories with branch policies, pull request code reviews, and semantic search.
3. **Azure Pipelines:** Cloud-native CI/CD supporting multi-stage YAML pipelines across Linux, macOS, and Windows.
4. **Azure Test Plans:** Manual, exploratory, and automated end-to-end quality assurance testing.
5. **Azure Artifacts:** Universal package management for NuGet, npm, Maven, Python PyPI, and Universal Packages.

### Why do Enterprises Choose Azure DevOps?
1. **Native Entra ID Identity & RBAC:** Single sign-on (SSO), Conditional Access policies, and fine-grained security groups inherited from corporate corporate directories.
2. **End-to-End Traceability:** Work items link directly to Git commits, pull requests, CI builds, release deployments, and production bugs.
3. **Platform Agnostic:** Builds and deploys any language (Java, Python, .NET, Go, Node.js) to any target (Azure, AWS, GCP, on-premises Kubernetes, mobile app stores).

### 📜 History & Evolution:
- **2005:** Microsoft releases **Team Foundation Server (TFS)** as an on-premises enterprise version control and project tracking suite.
- **2013:** Cloud version launches under the name **Visual Studio Online (VSO)**.
- **2015:** Rebranded to **Visual Studio Team Services (VSTS)**, introducing browser-based build systems and Git support.
- **2018:** Completely overhauled and rebranded as **Azure DevOps**, introducing modern YAML multi-stage pipelines and decoupled standalone service offerings.

---

## 🏗️ 2. The 3-Tier Azure DevOps Hierarchy

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE DEVOPS RESOURCE HIERARCHY                           │
│                                                                                        │
│   TIER 1: MICROSOFT ENTRA ID TENANT (Identity & Authentication Boundary)               │
│   (contoso.onmicrosoft.com)                                                            │
│                               │                                                        │
│                               ▼                                                        │
│   TIER 2: AZURE DEVOPS ORGANIZATION (Billing & Governance Boundary)                    │
│   (dev.azure.com/contoso-enterprise)                                                   │
│   • Global User Licensing (Basic vs. Stakeholder vs. Visual Studio Enterprise)         │
│   • Global Extension Marketplace, Organization-Level Agent Pools & Permissions         │
│                               │                                                        │
│                               ▼                                                        │
│   TIER 3: PROJECTS (Operational Collaboration Boundary)                                │
│   ┌───────────────────────────────────┬───────────────────────────────────┐            │
│   │ Project: Core-Banking-Platform    │ Project: Mobile-Banking-App       │            │
│   │ Process: Scrum                    │ Process: Agile                    │            │
│   │ • Private Git Repositories        │ • Private Git Repositories        │            │
│   │ • YAML CI/CD Pipelines            │ • YAML CI/CD Pipelines            │            │
│   │ • Service Connections to Azure    │ • Service Connections to Azure    │            │
│   │ • Package Feeds & Test Plans      │ • Package Feeds & Test Plans      │            │
│   └───────────────────────────────────┴───────────────────────────────────┘            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📋 3. Process Templates: Scrum vs. Agile vs. Basic vs. CMMI

When creating an Azure DevOps Project, you must choose an immutable work item process template:

| Process Template | Core Work Item Hierarchy | Best Suited For | Estimation Metric |
| :--- | :--- | :--- | :--- |
| **Basic** | Epic ➔ Issue ➔ Task | Simple projects, small teams wanting zero overhead | None (Checklist) |
| **Agile** | Epic ➔ Feature ➔ User Story ➔ Task / Bug | Standard modern software engineering teams | Story Points (Effort) |
| **Scrum** | Epic ➔ Feature ➔ Product Backlog Item (PBI) ➔ Task / Bug | Strict Scrum teams practicing 2-week sprint cadences | Story Points / Remaining Work |
| **CMMI** | Epic ➔ Feature ➔ Requirement ➔ Task / Bug / Change Request | Heavily regulated defense, aerospace, and banking orgs | Hours & Rigorous Change Audit |

---

## 💻 4. Practical CLI: Managing Azure DevOps with `az devops`

```bash
# 1. Install the Azure DevOps CLI extension
az extension add --name azure-devops

# 2. Configure default organization and login
az devops configure --defaults organization=https://dev.azure.com/contoso-cloud

# 3. Create a new private enterprise project using Agile process
az devops project create \
  --name "Payments-Modernization" \
  --description "Cloud-native microservices payments platform on AKS" \
  --process "Agile" \
  --visibility private

# 4. List projects and their current states
az devops project list --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Module Overview](./00-Overview.md) | [Index](../../../README.md) | [02 - Azure Repos and Branch Policies →](./02-Azure-Repos-and-Branch-Policies.md) |
