# 10 - Hands-On Practice: Git Basics and Three-Tree Lifecycle

## Lab Scenario
Initialize a local Git repository, practice staging, examine diffs across states, and safely unstage changes without data loss.

---

## Lab Steps

### Step 1: Initialize Repository and Configure Local Identity
```bash
mkdir /tmp/git-lab && cd /tmp/git-lab
git init
git config user.name "DevOps Engineer"
git config user.email "devops@company.internal"
```

### Step 2: Create Files and Observe Working Tree Status
```bash
echo "DATABASE_PORT=5432" > config.env
echo "# Project Documentation" > README.md
git status
```

### Step 3: Stage Only README.md
```bash
git add README.md
git status
# Notice README.md is under "Changes to be committed", config.env is "Untracked"
```

### Step 4: Inspect Differences
```bash
echo "Added line" >> README.md
# Inspect unstaged changes:
git diff
# Inspect staged changes:
git diff --staged
```

### Step 5: Commit and Verify Log
```bash
git add README.md
git commit -m "docs: add initial project documentation"
git log --oneline
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
