# 03 - Enforcing Signed Commits in Branch Protection

## 1. GitHub & GitLab "Verified" Badges

When a signed commit is pushed:
1. GitHub extracts the public signature from the commit object.
2. Matches it against the verified GPG/SSH keys registered on the user's account.
3. Displays the green **Verified** badge.

---

## 2. Enforcing Signed Commits in Enterprise Branch Rules

To prevent spoofed commits from entering production:
- In GitHub: Navigate to **Settings -> Branches -> Branch Protection Rules**.
- Check: **"Require signed commits"**.
- Any push containing an unsigned commit is immediately rejected by the server!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Cryptographic Signing](./02-Cryptographic-Signing-with-GPG-and-SSH-Keys.md) | [README](./README.md) | [04 - Purging Secrets](./04-Purging-Leaked-Secrets-with-git-filter-repo.md) |
