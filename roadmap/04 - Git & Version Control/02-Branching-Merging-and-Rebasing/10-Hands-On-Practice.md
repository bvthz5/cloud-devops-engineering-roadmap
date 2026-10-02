# 10 - Hands-On Practice: Branching, Merging, and Interactive Rebasing

## Lab Scenario
Create a feature branch, generate several iterative commits, use interactive rebase to squash them into a single clean conventional commit, and merge into main.

---

## Lab Steps

### Step 1: Create and Switch to Feature Branch
```bash
git checkout -b feature/auth
```

### Step 2: Create Iterative Development Commits
```bash
echo "def login():" > auth.py
git add auth.py && git commit -m "WIP: start login function"

echo "    return True" >> auth.py
git add auth.py && git commit -m "fix: complete login logic"

echo "# Tests" > test_auth.py
git add test_auth.py && git commit -m "test: add auth tests"
```

### Step 3: Interactively Rebase to Squash Commits
```bash
git rebase -i HEAD~3
```
In the editor, keep the first commit as `pick`, and change the next two to `fixup`:
```text
pick <SHA1> WIP: start login function
f <SHA2> fix: complete login logic
f <SHA3> test: add auth tests
```
Save and exit. Check `git log --oneline`—all three commits are now condensed into one!

### Step 4: Merge Cleanly into Main
```bash
git checkout main
git merge feature/auth
git log --oneline
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
