# 05 - Server-Side Hooks: pre-receive and post-receive

## 1. `pre-receive` Hook: The Enterprise Gatekeeper

Executes on the central Git server when someone pushes. It receives a list of references being pushed:
```text
<old-value> <new-value> <ref-name>
```
If the script exits non-zero, **all commits in that push are rejected completely**!

---

## 2. `post-receive` Hook: Lightweight Continuous Deployment

Executes after commits are accepted on the server. Used for bare-metal or VM deployments:

```bash
#!/usr/bin/env bash
# /var/repo/site.git/hooks/post-receive

TARGET_DIR="/var/www/production"
GIT_DIR="/var/repo/site.git"

echo "Deploying to production web root..."
git --work-tree=$TARGET_DIR --git-dir=$GIT_DIR checkout -f main
systemctl reload nginx
echo "Deployment Complete!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Secret Scanning](./04-Secret-Scanning-and-Credential-Leak-Prevention.md) | [README](./README.md) | [06 - Bypassing Hooks](./06-Bypassing-Hooks-and-Security-Trade-Offs.md) |
