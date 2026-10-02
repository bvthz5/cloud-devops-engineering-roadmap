# 08 — Configuration Security: Secret Management, GitLeaks, and SOPS

Accidental leakage of database passwords, AWS access keys, and API tokens into public or private Git repositories is one of the leading causes of enterprise cloud security breaches. This guide covers how to secure declarative configurations using modern secret management tools.

---

## 1. The Anatomy of Secret Leaks & Prevention

```text
Developer Commits Code ──► Git Pre-Commit Hook (GitLeaks / TruffleHog)
                                   │
                 ┌─────────────────┴─────────────────┐
                 ▼                                   ▼
        [ Secret Detected ]                 [ Clean Commit ]
        Commit is BLOCKED!                  Pushed to GitHub
        Token never leaves local disk       CI/CD executes safely
```

### Automated Secret Scanning Tools
1. **GitLeaks:** High-speed scanner that audits Git repositories for hardcoded secrets, API keys, and private certificates using regular expressions and Shannon entropy checks.
2. **TruffleHog:** Scans Git histories, S3 buckets, and Docker images for active, verifiable leaked credentials.

### Installing and Running GitLeaks
```bash
# Install GitLeaks via binary / Homebrew
brew install gitleaks
# Or download binary for Linux:
# curl -sSL https://github.com/gitleaks/gitleaks/releases/download/v8.18.0/gitleaks_8.18.0_linux_x64.tar.gz | tar -xz -C /usr/local/bin

# Scan current git repository for leaked credentials
gitleaks detect --verbose
```

---

## 2. Secrets in Git: The Three Production Paradigms

How do you manage Kubernetes Secret manifests in Git version control without exposing raw credentials?

```text
+-------------------------------------------------------------+
|               Modern Secrets Management Paradigms           |
+-------------------------------------------------------------+
| Paradigm 1: Encrypted Files in Git (Mozilla SOPS)           |
|   Secrets are encrypted with AWS KMS / PGP / Age before     |
|   being committed. Safe to store directly in Git repository!|
|                                                             |
| Paradigm 2: In-Cluster Sealed Secrets (Bitnami)             |
|   Asymmetric encryption; only the target Kubernetes cluster |
|   private key can decrypt the SealedSecret resource.        |
|                                                             |
| Paradigm 3: External Secret Store (HashiCorp Vault / ESO)   |
|   Secrets live outside Git in Vault or AWS Secrets Manager; |
|   an in-cluster operator dynamically fetches them at runtime|
+-------------------------------------------------------------+
```

---

## 3. Deep Dive: Mozilla SOPS (Secrets OPerationS)

**Mozilla SOPS** is an open-source editor of encrypted files that supports YAML, JSON, ENV, INI, and BINARY formats.

### The SOPS Breakthrough: Value-Level Encryption
Traditional encryption tools (like GPG or OpenSSL) encrypt the entire file into a binary blob, destroying Git diffs.
**SOPS encrypts only the values, leaving the keys in plaintext!**

```yaml
# Plaintext secret (Never commit this!):
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
data:
  password: super_secret_production_password

# SOPS-Encrypted File (Safe to commit to Git!):
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
data:
  password: ENC[AES256_GCM,data:w8a3...==,iv:jK...,tag:9Q...==]
sops:
  kms:
    - arn: arn:aws:kms:us-east-1:123456789012:key/abcd-1234
  lastmodified: '2026-10-02T07:30:00Z'
```

### Working with SOPS via CLI
```bash
# 1. Encrypt a YAML file using an AWS KMS Key
sops --encrypt \
  --kms "arn:aws:kms:us-east-1:123456789012:key/my-key-id" \
  secret.yaml > secret.enc.yaml

# 2. Decrypt on the fly and pipe directly to kubectl (Zero disk plaintext exposure!)
sops --decrypt secret.enc.yaml | kubectl apply -f -

# 3. Interactively edit encrypted file in Vim (SOPS decrypts to memory and re-encrypts on save)
sops secret.enc.yaml
```

---

## 4. External Secrets Operator (ESO) for Kubernetes

The industry-standard architecture for modern enterprise Kubernetes is the **External Secrets Operator (ESO)**:

```text
Cloud Secrets Store (AWS Secrets Manager / HashiCorp Vault)
                               │
                               ▼ Synchronizes via API
Kubernetes External Secrets Operator (In-Cluster Controller)
                               │
                               ▼ Generates in-memory
Native Kubernetes Secret (k8s v1/Secret mounted into Pod)
```

### Sample ExternalSecret Custom Resource:
```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: database-credentials
spec:
  refreshInterval: "1h"
  secretStoreRef:
    name: aws-secrets-manager
    kind: ClusterSecretStore
  target:
    name: db-secret-k8s # Target native Kubernetes Secret created
  data:
    - secretKey: password
      remoteRef:
        key: "prod/payments/db"
        property: "password"
```

---

## 5. Secret Security Anti-Patterns Checklist

- ❌ **Base64 is NOT Encryption:** Kubernetes `Secret` data is only base64-encoded. Anyone who can read the YAML can decode it in 1 second (`echo "..." | base64 -d`).
- ❌ **Never Commit `.env` Files:** Add `.env` and `*.key` to `.gitignore` by default.
- ❌ **Never Pass Secrets as Docker Build Args:** `ARG SECRET=xyz` is permanently baked into the intermediate image layers viewable with `docker history`. Use **BuildKit Secrets Mounts** (`--mount=type=secret`) instead.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Config Management Environments and Templating](./07-Config-Management-Environments-and-Templating.md) | [README](./README.md) | [09 - Real World Scenarios](./09-Real-World-Scenarios.md) |
