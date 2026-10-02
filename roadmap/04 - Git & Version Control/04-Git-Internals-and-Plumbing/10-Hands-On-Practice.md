# 10 - Hands-On Practice: Building a Commit with Plumbing Commands

## Lab Scenario
Create a complete, valid Git commit from scratch without running `git add` or `git commit`.

---

## Lab Steps

### Step 1: Create a Blob Object
```bash
mkdir /tmp/plumbing-lab && cd /tmp/plumbing-lab && git init
BLOB_SHA=$(echo "echo 'Hello from low-level Git!'" | git hash-object -w --stdin)
echo "Created Blob: $BLOB_SHA"
```

### Step 2: Add Blob to Index
```bash
git update-index --add --cacheinfo 100644 $BLOB_SHA script.sh
```

### Step 3: Write Index to a Tree Object
```bash
TREE_SHA=$(git write-tree)
echo "Created Tree: $TREE_SHA"
git ls-tree $TREE_SHA
```

### Step 4: Create Commit Object Pointing to Tree
```bash
COMMIT_SHA=$(echo "feat: manually crafted plumbing commit" | git commit-tree $TREE_SHA)
echo "Created Commit: $COMMIT_SHA"
git cat-file -p $COMMIT_SHA
```

### Step 5: Update Main Branch Reference
```bash
git update-ref refs/heads/main $COMMIT_SHA
git log --oneline
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
