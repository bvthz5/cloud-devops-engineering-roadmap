# 05 - Decompiling ARM Templates & Bicep CLI Tooling

> **Focus:** Modernization and toolchain workflows in Azure Bicep. Deep dive into decompiling legacy ARM JSON templates into Bicep (`bicep decompile`), exporting live Azure resources, Bicep Linter code quality enforcement, and Bicep CLI commands.

---

## 🔄 1. What, Why, and History of ARM Decompilation

### What is ARM Decompilation?
Decompilation is the automated reverse-engineering process where the Bicep CLI reads an existing legacy ARM JSON template, parses its syntax tree, and converts it into clean, idiomatic Azure Bicep code files.

### Why do Enterprise Teams Need Decompilation?
1. **Legacy Modernization:** Organizations with thousands of legacy ARM templates authored between 2014 and 2020 can migrate to Bicep in minutes rather than spending months rewriting JSON by hand.
2. **Reverse-Engineering Portal Deployments:** When engineers test a complex architecture in the Azure Portal GUI and export the deployment template, decompiling converts that messy JSON into clean, reusable Bicep code.
3. **Automated Linter Remediation:** The Bicep compiler identifies anti-patterns in legacy code (e.g., hardcoded passwords or location strings) and flags them during conversion.

---

## 🛠️ 2. Step-by-Step: Decompiling ARM JSON to Bicep

```bash
# 1. Decompile an existing ARM JSON file into Bicep
az bicep decompile --file legacy-azuredeploy.json

# The Bicep CLI analyzes expressions, strips [concat()] boilerplate,
# and generates: legacy-azuredeploy.bicep
```

### What the Decompiler Converts Automatically:
- Converts `[concat('storage', uniqueString(resourceGroup().id))]` ➔ `'storage${uniqueString(resourceGroup().id)}'`
- Converts `[parameters('adminUsername')]` ➔ `adminUsername`
- Converts `[resourceId('Microsoft.Network/virtualNetworks', 'vnet-prod')]` ➔ Direct symbolic resource reference `vnet.id`

---

## 🧹 3. Bicep Linter: Enforcing Code Quality & Security

The Bicep compiler includes a built-in static analysis **Linter** configured via `bicepconfig.json`. It runs automatically during authoring in VS Code and during CI/CD build execution:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              KEY BICEP LINTER SECURITY RULES                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ • no-hardcoded-env-urls: Flags hardcoded URLs like 'management.azure.com'              │
│ • secure-secrets-in-params: Flags parameters with passwords lacking @secure()          │
│ • simplify-interpolation: Replaces [concat('a', 'b')] with 'ab'                       │
│ • no-unused-params: Identifies dead parameters cluttering templates                   │
│ • use-resource-id-functions: Enforces safe dynamic resource ID calculation             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Comprehensive Bicep CLI Command Cheat Sheet

```bash
# Compile Bicep to ARM JSON
az bicep build --file main.bicep

# Reverse decompile ARM JSON to Bicep
az bicep decompile --file azuredeploy.json

# Automatically format Bicep files to consistent indentation
az bicep format --file main.bicep

# Upgrade Bicep CLI to latest version
az bicep upgrade

# Run static analysis and linting checks
az bicep lint --file main.bicep
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Target Scopes ResourceGroup Subscription ManagementGroup](./04-Target-Scopes-ResourceGroup-Subscription-ManagementGroup.md) | [Index](../../../README.md) | [06 - Bicep Private Registries and Template Specs →](./06-Bicep-Private-Registries-and-Template-Specs.md) |
