# 06 - Azure Artifacts: Universal Package Management & Upstream Caching

> **Focus:** Software supply chain security and package sharing in Azure DevOps. Deep dive into Azure Artifacts feeds, supported ecosystems (npm, NuGet, Maven, Python PyPI, Universal Packages), Upstream Sources caching, Feed Views (`@prerelease`, `@release`), and dependency confusion defense.

---

## 📦 1. What, Why, and History of Azure Artifacts

### What is Azure Artifacts?
Azure Artifacts is a fully managed enterprise package management service that allows development teams to publish, share, version, and consume binary packages and proprietary code libraries across multiple repositories and CI/CD pipelines.

### Why do Modern Engineering Organizations Use Private Artifact Feeds?
1. **Proprietary Code Sharing:** Enterprise shared libraries (e.g., company authentication middleware, common logging libraries, banking SDKs) must be shared internally across 50 development teams without ever being leaked to public registries (like npmjs.org or nuget.org).
2. **Protection Against Upstream Deletions (Left-Pad Protection):** Public registries can suffer outages or package authors can unpublish libraries. Azure Artifacts automatically caches a copy of every public dependency consumed, ensuring your CI/CD builds will never fail even if the public package is deleted or compromised.
3. **Defense Against Dependency Confusion Attacks:** Prevents malicious attackers from registering identical internal package names on public registries by enforcing upstream source priority.

### 📜 History & Evolution:
- **2015:** Introduced as the "Package Management" extension for Visual Studio Team Services.
- **2018:** Rebranded to **Azure Artifacts**, expanding beyond NuGet to first-class native support for npm, Maven/Gradle, and Python wheels/tarballs.
- **2019:** Introduced **Universal Packages**—allowing multi-gigabyte build artifacts, ML models, and compiled binaries to be stored and versioned efficiently.

---

## ⚖️ 2. Supported Package Ecosystems & Protocols

| Ecosystem | Protocol / Tooling | Target Languages | Common File Extensions |
| :--- | :--- | :--- | :--- |
| **npm** | `npm`, `yarn`, `pnpm` | JavaScript, TypeScript, Node.js | `.tgz` |
| **NuGet** | `dotnet nuget`, `nuget.exe` | C#, F#, VB.NET, .NET Core | `.nupkg`, `.snupkg` |
| **Maven / Gradle** | `mvn`, `gradle` | Java, Kotlin, Scala, Groovy | `.jar`, `.war`, `.pom` |
| **Python** | `pip`, `twine`, `poetry` | Python 3.x | `.whl`, `.tar.gz` |
| **Universal Packages** | `az artifacts universal` | Raw binaries, game assets, ML models, ISOs | Arbitrary binary files |

---

## 🛡️ 3. Upstream Sources & Feed Views Architecture

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE ARTIFACTS FEED TOPOLOGY                             │
│                                                                                        │
│   DEVELOPER WORKSTATION OR PIPELINE RUN                                                │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ Client builds project: runs 'npm install' or 'dotnet restore'                   │   │
│   │ Configured .npmrc / nuget.config points strictly to:                           │   │
│   │ https://pkgs.dev.azure.com/contoso/_packaging/Corporate-Core-Feed/npm/registry/│   │
│   └──────────────────────────────────────┬─────────────────────────────────────────┘   │
│                                          │                                             │
│                                          ▼                                             │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ AZURE ARTIFACTS PRIVATE FEED (Corporate-Core-Feed)                             │   │
│   │                                                                                │   │
│   │ ┌──────────────────────────┐         ┌───────────────────────────────────────┐ │   │
│   │ │ Internal Custom Packages │         │ Cached External Upstream Packages     │ │   │
│   │ │ • @contoso/auth: 2.1.0   │         │ • express: 4.19.2 (Cached copy)       │ │   │
│   │ │ • @contoso/logger: 1.4.0 │         │ • lodash: 4.17.21 (Cached copy)       │ │   │
│   │ └──────────────────────────┘         └───────────────────▲───────────────────┘ │   │
│   └──────────────────────────────────────────────────────────┼─────────────────────┘   │
│                                                              │ (Cache on first fetch)  │
│                                                              │                         │
│                                                  ┌───────────┴───────────┐             │
│                                                  │ PUBLIC REGISTRIES     │             │
│                                                  │ • npmjs.org           │             │
│                                                  │ • nuget.org           │             │
│                                                  │ • pypi.org            │             │
│                                                  └───────────────────────┘             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🏷️ Feed Views Quality Promotion:
Feeds use **Views** to indicate package stability without changing package version numbers:
- `@local`: Every package built by developers and CI builds.
- `@prerelease`: Packages validated by automated testing; ready for QA teams.
- `@release`: High-confidence, production-vetted libraries approved for mission-critical deployments.

---

## 💻 4. Practical Implementation: Publishing Packages in Azure Pipelines

```yaml
# Pipeline snippet publishing an npm package to an Azure Artifacts Feed
trigger: none

jobs:
  - job: PublishPackage
    pool:
      vmImage: 'ubuntu-latest'
    steps:
      - checkout: self

      - task: NodeTool@0
        inputs:
          versionSpec: '20.x'
        displayName: 'Install Node 20'

      - task: NpmAuthenticate@0
        displayName: 'Authenticate with Azure Artifacts Feed'
        inputs:
          workingFile: '$(Build.SourcesDirectory)/.npmrc'

      - script: |
          npm ci
          npm run build
          npm publish
        displayName: 'Compile and Publish Private Package'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Service Connections Variables and Environments](./05-Service-Connections-Variables-and-Environments.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
