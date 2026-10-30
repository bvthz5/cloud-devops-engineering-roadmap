# 02 - Cryptographic Signing with GPG and SSH Keys

## 1. Modern SSH Key Signing (Git 2.34+)

While GPG is notoriously complex to configure, modern Git supports signing commits directly with your existing **SSH Key**:

```bash
# Step 1: Tell Git to use SSH for signing
git config --global gpg.format ssh

# Step 2: Specify your SSH public key
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Step 3: Enable auto-signing for every commit
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

---

## 2. GPG Key Signing

```bash
# Generate GPG key
gpg --full-generate-key

# List secret keys to get Key ID
gpg --list-secret-keys --keyid-format=long

# Configure Git with GPG Key ID
git config --global user.signingkey 3AA5C34371567BD2
git config --global commit.gpgsign true

# Sign a commit manually
git commit -S -m "feat: cryptographically signed commit"
```

---

## 3. Verifying Signatures Locally
```bash
# Show commits with signature verification status
git log --show-signature
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Commit Spoofing Threat Model and Identity](./01-Commit-Spoofing-Threat-Model-and-Identity.md) | [Index](../../../README.md) | [03 - Enforcing Signed Commits in Branch Protection →](./03-Enforcing-Signed-Commits-in-Branch-Protection.md) |
