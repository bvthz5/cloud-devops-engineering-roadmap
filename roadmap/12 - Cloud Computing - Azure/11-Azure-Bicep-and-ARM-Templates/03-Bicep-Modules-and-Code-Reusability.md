# 03 - Bicep Modules & Code Reusability

```bicep
module vnetModule 'modules/vnet.bicep' = {
  name: 'vnetDeployment'
  params: {
    vnetName: 'vnet-prod-eastus'
    addressPrefix: '10.0.0.0/16'
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Bicep Syntax](./02-Bicep-Language-Syntax-Parameters-Variables-and-Outputs.md) | [README](./README.md) | [04 - Target Scopes](./04-Target-Scopes-ResourceGroup-Subscription-ManagementGroup.md) |
