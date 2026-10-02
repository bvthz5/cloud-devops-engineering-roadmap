# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Accidental Root Directory Deletion (`rm -rf "$DIR"/*`)

### Context & Incident
A company automated daily cache purges across 300 production worker VMs using a Bash script executed as a root cron job. One morning, 300 instances became completely unresponsive simultaneously.

### Root Cause
The script contained:
```bash
PURGE_TARGET="/var/cache/app"
# ... script logic that redefined or cleared the variable under certain conditions ...
rm -rf $PURGE_TARGET/*
```
Because of an unhandled exit code in an earlier step, `PURGE_TARGET` evaluated to an empty string. The unquoted statement executed as:
`rm -rf /*`
wiping the entire root filesystem of all 300 production worker nodes.

### Prevention & Testing
1. **ShellCheck Rule SC2115 / SC2086**: ShellCheck immediately flags `rm -rf $VAR/*` if `$VAR` could be unset.
2. **Bash Parameter Expansion Guards**:
```bash
# Refuses to execute if variable is empty or unset
rm -rf "${PURGE_TARGET:?Variable PURGE_TARGET is unset or empty}"/*
```
3. **Bats Automated Tests**: Enforce unit tests that simulate empty parameters and assert that the script terminates immediately with status code > 0.

---

## Scenario 2: Mock Drift Causing Cloud Outage

### Context & Incident
An engineer wrote a Python script to enforce MFA on all AWS IAM users. The script had 100% unit test coverage using custom handcrafted mocks (`unittest.mock.MagicMock`). When deployed to production, the script failed with:
`AttributeError: 'dict' object has no attribute 'mfa_devices'`

### Root Cause
The developer mocked `iam.get_user()` as returning an object with an `.mfa_devices` attribute. However, the real AWS Boto3 API returns a Python dictionary where MFA devices must be queried via a completely separate API call (`iam.list_mfa_devices()`).

### Architectural Solution
Avoid handcrafted `MagicMock` objects for cloud APIs. Use **`moto`**, which emulates the exact request/response dictionary schemas of real AWS wire endpoints.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Mutation Testing & Resilience](./06-Mutation-Testing-and-Resilience-Validation.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
