# 02 - Bicep Language Syntax

```bicep
@description('The location for all resources.')
param location string = resourceGroup().location

@secure()
param adminPassword string

var storageAccountName = 'st${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}

output storageEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - ARM vs Bicep](./01-Infrastructure-as-Code-on-Azure-ARM-vs-Bicep.md) | [README](./README.md) | [03 - Bicep Modules](./03-Bicep-Modules-and-Code-Reusability.md) |
