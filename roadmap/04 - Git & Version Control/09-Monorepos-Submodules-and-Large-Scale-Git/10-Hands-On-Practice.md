# 10 - Hands-On Practice: Monorepo Sparse Checkout and Blobless Clones

## Lab Scenario
Configure a sparse checkout in cone mode on a simulated monorepo with multiple service directories.

---

## Lab Steps

### Step 1: Create Monorepo Structure
```bash
mkdir -p /tmp/monorepo/services/auth /tmp/monorepo/services/billing /tmp/monorepo/libs/common
cd /tmp/monorepo && git init
echo "auth code" > services/auth/auth.go
echo "billing code" > services/billing/billing.go
echo "common code" > libs/common/common.go
git add . && git commit -m "feat: initial monorepo layout"
```

### Step 2: Initialize Sparse Checkout in Cone Mode
```bash
git sparse-checkout init --cone
```

### Step 3: Set Sparse Checkout to Single Service
```bash
git sparse-checkout set services/auth
ls -la
# Notice services/billing and libs/common have disappeared from disk!
```

### Step 4: Add Shared Library
```bash
git sparse-checkout add libs/common
ls -la libs/
# libs/common is now visible!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
