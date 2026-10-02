# 08 - Troubleshooting Azure CLI & Subscriptions

| Error | Cause | Fix |
|---|---|---|
| `The subscription 'xxx' could not be found` | Wrong account or context. | Run `az login` and `az account set --subscription <id>`. |
| `MissingSubscriptionRegistration` | Resource provider not registered. | Run `az provider register --namespace Microsoft.Compute`. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
