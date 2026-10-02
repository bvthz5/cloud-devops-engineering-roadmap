# 10 - Hands-On Practice: Remotes, Upstream Sync, and CODEOWNERS

## Lab Scenario
Configure multiple remotes (`origin` and `upstream`), practice fetching and tracking branches, and draft a production `CODEOWNERS` file.

---

## Lab Steps

### Step 1: Configure Multiple Remotes
```bash
git remote add origin git@github.com:my-user/my-fork.git
git remote add upstream git@github.com:enterprise-org/core-platform.git
git remote -v
```

### Step 2: Create a Production CODEOWNERS Manifest
```bash
mkdir -p .github
cat << 'EOF' > .github/CODEOWNERS
# Global Platform Governance
*                   @devops-lead

# Cloud Infrastructure & Kubernetes
/terraform/         @cloud-engineers
/kubernetes/        @cloud-engineers

# Security & Secrets Management
*.pem               @security-team
.env*               @security-team
EOF
git add .github/CODEOWNERS
git commit -m "chore: add production CODEOWNERS configuration"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
