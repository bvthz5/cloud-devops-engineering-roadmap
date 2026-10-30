# Hands-On Practice - GitLab CI Pipelines & Runners

> **Module**: GitLab CI Pipelines & Runners

---

## 🛠 Lab: Building a Production-Grade GitHub Actions Pipeline

### Objective
In this lab, you will write a complete GitHub Actions workflow featuring dependency caching, matrix builds across multiple Node.js versions, and artifact upload.

---

## 📋 Task 1: Create Workflow File (`.github/workflows/build.yml`)
```yaml
name: Production Matrix Pipeline
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  matrix-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]

    steps:
    - uses: actions/checkout@v3

    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'

    - name: Install & Test
      run: |
        npm ci
        npm test --if-present
```

---

## 📋 Task 2: Add Build Artifact Upload Step
```yaml
    - name: Build Project
      run: npm run build --if-present

    - name: Upload Build Artifact
      uses: actions/upload-artifact@v3
      with:
        name: dist-build-${{ matrix.node-version }}
        path: dist/
```

---

## 📋 Task 3: Test Local Execution with `act`
```bash
# Test GitHub Actions workflow locally using act CLI
act pull_request
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
