# 05 - Decompiling ARM Templates & What-If Dry Run

```bash
# Decompile ARM JSON to Bicep
az bicep decompile --file template.json

# Execute dry run preview
az deployment group create --resource-group rg-demo --template-file main.bicep --what-if
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Target Scopes ResourceGroup Subscription ManagementGroup](./04-Target-Scopes-ResourceGroup-Subscription-ManagementGroup.md) | [Index](../../../README.md) | [06 - Bicep Private Registries and Template Specs →](./06-Bicep-Private-Registries-and-Template-Specs.md) |
