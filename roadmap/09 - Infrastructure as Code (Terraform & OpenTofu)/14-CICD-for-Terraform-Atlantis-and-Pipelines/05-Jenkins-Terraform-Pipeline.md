# 05 - Jenkins Terraform Pipeline

```groovy
pipeline {
    agent any
    stages {
        stage('Init') {
            steps { sh 'terraform init' }
        }
        stage('Plan') {
            steps { sh 'terraform plan -out=tfplan' }
        }
        stage('Approval') {
            steps {
                input message: 'Apply this plan?', ok: 'Apply'
            }
        }
        stage('Apply') {
            steps { sh 'terraform apply tfplan' }
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Atlantis](./04-Atlantis-PR-Based-Terraform-Automation.md) | [README](./README.md) | [06 - Pipeline Security](./06-Pipeline-Security-OIDC-and-Ephemeral-Credentials.md) |
