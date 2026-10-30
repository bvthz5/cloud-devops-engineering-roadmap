# 08 - Troubleshooting

## Common Provisioner Errors

| Error | Cause | Fix |
|---|---|---|
| `timeout - last error: dial tcp: connect` | SSH not reachable | Check security groups, key pair, and public IP |
| `Permission denied (publickey)` | Wrong SSH key or user | Verify `user` and `private_key` in connection block |
| `Error running command: exit status 1` | Script failed | Check script output; add `set -e` to bash scripts |
| `Resource tainted due to provisioner failure` | Provisioner failed on creation | Fix script, then `terraform untaint` or `apply -replace` |
| `WinRM connection timeout` | WinRM not enabled on instance | Enable WinRM via user_data before provisioner runs |

## Debugging Provisioners

```bash
# Enable detailed logging
TF_LOG=DEBUG terraform apply 2> provisioner-debug.log

# Test SSH connectivity manually before using remote-exec
ssh -i ~/.ssh/key.pem ubuntu@<IP> 'echo SUCCESS'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
