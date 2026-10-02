# 10 - Hands-On Practice: Trunk-Based Development Workflow

## Lab Scenario
Simulate a Trunk-Based Development flow using short-lived branches, squashed merges, and annotated release tagging.

---

## Lab Steps

### Step 1: Create Main Branch and Baseline
```bash
git checkout -b main
echo "API Version 1.0" > app.py
git add app.py && git commit -m "feat: initial API setup"
git tag -a v1.0.0 -m "Release v1.0.0"
```

### Step 2: Implement Feature on Short-Lived Branch
```bash
git checkout -b feature/health-endpoint
echo "def health(): return 200" >> app.py
git add app.py && git commit -m "feat(api): add health check endpoint"
```

### Step 3: Rebase and Fast-Forward Merge into Main
```bash
git checkout main
git merge --ff-only feature/health-endpoint
git branch -d feature/health-endpoint
```

### Step 4: Tag Milestone Release
```bash
git tag -a v1.1.0 -m "Release v1.1.0: Added health check"
git tag -l -n9
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
