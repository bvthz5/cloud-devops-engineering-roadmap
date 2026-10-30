# 09 - Git Security: Interview Questions & Answers

### Q1: Why is relying on Git `user.name` and `user.email` a major security risk?
**Answer:** Because `user.name` and `user.email` are arbitrary string metadata set locally in `.gitconfig`. Anyone can set these values to impersonate an executive, team lead, or trusted maintainer without authentication. Cryptographic commit signing (GPG/SSH) is required to guarantee authentic authorship.

### Q2: Why should teams use `git filter-repo` instead of the legacy `git filter-branch` to purge secrets?
**Answer:** `git filter-branch` is deprecated by Git maintainers because it is extraordinarily slow, has dangerous edge-case bugs that can corrupt history, and leaves unpruned reference relics. `git filter-repo` is Python-based, orders of magnitude faster, and handles ref updates and packfile cleanups automatically.

### Q3: What is the benefit of SSH commit signing over GPG?
**Answer:** Developers already possess and understand SSH keys for GitHub/server authentication. GPG involves complex key management, expiration issues, and external daemons (`gpg-agent`). Modern Git (2.34+) allows using existing SSH keys (`id_ed25519.pub`) directly to sign commits.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
