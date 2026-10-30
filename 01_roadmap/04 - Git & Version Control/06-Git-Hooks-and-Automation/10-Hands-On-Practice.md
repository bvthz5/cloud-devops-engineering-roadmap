# 10 - Hands-On Practice: Building an Automated Secret Detection Hook

## Lab Scenario
Create an executable client-side `pre-commit` hook that scans staged files for leaked AWS credentials and blocks commits containing keys.

---

## Lab Steps

### Step 1: Create the Hook Script
```bash
cat << 'EOF' > .git/hooks/pre-commit
#!/usr/bin/env bash

# Scan staged diff for AWS access keys
if git diff --cached | grep -qE 'AKIA[0-9A-Z]{16}'; then
    echo "======================================================"
    echo "SECURITY ALERT: Detected potential AWS Access Key ID!"
    echo "Commit aborted to prevent credential leakage."
    echo "======================================================"
    exit 1
fi
exit 0
EOF
chmod +x .git/hooks/pre-commit
```

### Step 2: Test Legitimate Commit (Should Pass)
```bash
echo "DATABASE_NAME=production" > config.txt
git add config.txt
git commit -m "chore: add db config"
# Output: [main ...]: chore: add db config
```

### Step 3: Test Leaked Key Commit (Should Be Blocked!)
```bash
echo "AWS_KEY=AKIAIOSFODNN7EXAMPLE" >> config.txt
git add config.txt
git commit -m "test leak"
# Notice the hook halts execution with exit code 1!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
