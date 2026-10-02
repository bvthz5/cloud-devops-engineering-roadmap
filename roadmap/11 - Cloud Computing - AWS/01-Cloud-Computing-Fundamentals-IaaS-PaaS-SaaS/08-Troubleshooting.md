# 08 - Troubleshooting AWS CLI & Credentials

| Error | Root Cause | Fix |
|---|---|---|
| `Unable to locate credentials` | Credentials missing in `~/.aws/credentials`. | Run `aws configure` or export `AWS_ACCESS_KEY_ID`. |
| `The security token included in the request is invalid` | Expired temporary credentials or wrong profile. | Re-authenticate via `aws sso login` or update session token. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
