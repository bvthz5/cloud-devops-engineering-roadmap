# 10 - Hands-On Practice: Configuring SSH Key Commit Signing

## Lab Scenario
Configure your local Git client to sign commits using an existing Ed25519 SSH key and verify the signature in the git log.

---

## Lab Steps

### Step 1: Generate an Ed25519 Keypair (if not present)
```bash
ssh-keygen -t ed25519 -C "devops-signing-key" -f ~/.ssh/git_signing_key -N ""
```

### Step 2: Configure Git to Use SSH Signing
```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/git_signing_key.pub
git config --global commit.gpgsign true
```

### Step 3: Create a Signed Commit
```bash
echo "secure code" > secure.py
git add secure.py
git commit -m "feat: cryptographically signed commit via SSH"
```

### Step 4: Verify the Signature
```bash
git log --show-signature -n 1
# Look for: Good "git" signature for devops-signing-key with ED25519 key...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
