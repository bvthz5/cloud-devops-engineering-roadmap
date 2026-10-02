# 02 - GitHub Actions for Terraform

```yaml
name: Terraform
on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  id-token: write     # OIDC
  contents: read
  pull-requests: write # Comment on PRs

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789:role/GitHubActions
          aws-region: us-east-1

      - uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.7.0

      - name: Init
        run: terraform init

      - name: Format Check
        run: terraform fmt -check

      - name: Plan
        id: plan
        run: terraform plan -no-color -out=tfplan
        continue-on-error: true

      - name: Comment Plan on PR
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v7
        with:
          script: |
            const output = `#### Terraform Plan
            \`\`\`
            ${{ steps.plan.outputs.stdout }}
            \`\`\``;
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: output
            });

      - name: Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve tfplan
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terraform CI CD Pipeline Architecture](./01-Terraform-CI-CD-Pipeline-Architecture.md) | [Index](../../../README.md) | [03 - GitLab CI for Terraform →](./03-GitLab-CI-for-Terraform.md) |
