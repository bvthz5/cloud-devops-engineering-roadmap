# 04 - Creating Custom RBAC Roles

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can restart and monitor virtual machines",
  "Actions": [
    "Microsoft.Compute/virtualMachines/read",
    "Microsoft.Compute/virtualMachines/restart/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Azure RBAC Architecture](./03-Azure-Role-Based-Access-Control-RBAC-Architecture.md) | [README](./README.md) | [05 - Managed Identities](./05-Managed-Identities-System-Assigned-vs-User-Assigned.md) |
