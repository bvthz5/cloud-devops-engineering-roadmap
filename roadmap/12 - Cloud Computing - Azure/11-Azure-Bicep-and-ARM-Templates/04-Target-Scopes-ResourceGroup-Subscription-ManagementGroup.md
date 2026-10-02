# 04 - Target Scopes

Bicep files default to `resourceGroup` scope, but can target `subscription`, `managementGroup`, or `tenant`:
```bicep
targetScope = 'subscription'

resource newRG 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: 'rg-bicep-demo'
  location: 'eastus'
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Bicep Modules and Code Reusability](./03-Bicep-Modules-and-Code-Reusability.md) | [Index](../../../README.md) | [05 - Decompiling ARM Templates and Bicep CLI →](./05-Decompiling-ARM-Templates-and-Bicep-CLI.md) |
