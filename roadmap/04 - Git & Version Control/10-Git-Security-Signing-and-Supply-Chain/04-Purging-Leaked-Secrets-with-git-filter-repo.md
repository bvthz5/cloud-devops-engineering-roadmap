# 04 - Purging Leaked Secrets with git filter-repo

## 1. Why `git rm` Fails to Delete Secrets

Running `git rm passwords.txt && git commit` merely records that the file was deleted in the newest snapshot. **The password remains permanently readable in past commit objects!**

---

## 2. Using `git filter-repo` (The Modern Standard)

The legacy `git filter-branch` is deprecated due to extreme slowness and corruption bugs.
`git filter-repo` is Python-based, blazing fast, and officially recommended by the Git core team.

```bash
# Install git-filter-repo
pip install git-filter-repo

# Completely scrub a file from ALL commits in entire repository history:
git filter-repo --path passwords.txt --invert-paths

# Replace an API key string everywhere across all commits and files:
echo "AKIAIOSFODNN7EXAMPLE===>REDACTED_AWS_KEY" > replace.txt
git filter-repo --replace-text replace.txt

# Force push scrubbed history to remote
git push origin --force --all
git push origin --force --tags
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Enforcing Signed Commits in Branch Protection](./03-Enforcing-Signed-Commits-in-Branch-Protection.md) | [Index](../../../README.md) | [05 - Supply Chain Security SLSA Framework and SBOMs →](./05-Supply-Chain-Security-SLSA-Framework-and-SBOMs.md) |
