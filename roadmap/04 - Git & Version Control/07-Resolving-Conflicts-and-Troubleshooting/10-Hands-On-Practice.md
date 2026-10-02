# 10 - Hands-On Practice: Automated Conflict Resolution and Bisect

## Lab Scenario
Simulate a merge conflict, configure `merge.conflictstyle diff3`, and practice recovering a hard-reset commit with `git reflog`.

---

## Lab Steps

### Step 1: Configure diff3
```bash
git config merge.conflictstyle diff3
```

### Step 2: Create Conflicting Commits
```bash
git checkout -b branch-a
echo "SERVER_PORT=8000" > app.env
git add app.env && git commit -m "feat: use port 8000"

git checkout main
echo "SERVER_PORT=9000" > app.env
git add app.env && git commit -m "feat: use port 9000"
```

### Step 3: Trigger Conflict and Inspect diff3
```bash
git merge branch-a
cat app.env
# Notice <<<<<<<, |||||||, and >>>>>>> markers!
```

### Step 4: Resolve and Abort
```bash
git merge --abort
```

### Step 5: Test Reflog Recovery
```bash
git reset --hard HEAD~1
git reflog
# Note HEAD@{1} has previous commit!
git reset --hard HEAD@{1}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
