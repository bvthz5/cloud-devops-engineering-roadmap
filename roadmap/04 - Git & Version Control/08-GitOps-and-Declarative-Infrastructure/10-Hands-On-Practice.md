# 10 - Hands-On Practice: Encrypting Secrets with SOPS

## Lab Scenario
Install `sops` and `age` (modern cryptographic key tool) and encrypt a Kubernetes Secret file for safe storage in a public or shared Git repository.

---

## Lab Steps

### Step 1: Install age and SOPS
```bash
# On Linux:
sudo apt install -y age
# Or download binary for sops
```

### Step 2: Generate Age Keypair
```bash
age-keygen -o key.txt
export SOPS_AGE_KEY_FILE=$(pwd)/key.txt
AGE_PUBKEY=$(grep "public key:" key.txt | cut -d: -f2 | tr -d ' ')
```

### Step 3: Create Plaintext Kubernetes Secret
```yaml
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: database-credentials
data:
  password: SuperSecretProductionPassword123
```

### Step 4: Encrypt with SOPS
```bash
sops --encrypt --age $AGE_PUBKEY secret.yaml > secret.enc.yaml
cat secret.enc.yaml
# Notice the password value is encrypted with AES-256 GCM!
```

### Step 5: Decrypt and View
```bash
sops --decrypt secret.enc.yaml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
