# 06 - Bicep Private Registries & Template Specs

Publish reusable Bicep modules to Azure Container Registry (ACR):
```bash
az bicep publish --file modules/vnet.bicep --target 'br:myacr.azurecr.io/bicep/modules/vnet:v1'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Decompiling ARM Templates and Bicep CLI](./05-Decompiling-ARM-Templates-and-Bicep-CLI.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
