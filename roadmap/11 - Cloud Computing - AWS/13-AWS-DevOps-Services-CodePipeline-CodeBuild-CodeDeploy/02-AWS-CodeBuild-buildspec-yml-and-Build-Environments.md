# 02 - AWS CodeBuild & `buildspec.yml`

```yaml
version: 0.2
phases:
  install:
    commands:
      - echo Installing dependencies...
  pre_build:
    commands:
      - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URI
  build:
    commands:
      - docker build -t $ECR_URI:latest .
  post_build:
    commands:
      - docker push $ECR_URI:latest
artifacts:
  files:
    - imagedefinitions.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - AWS DevOps Overview](./01-AWS-DevOps-Services-Overview.md) | [README](./README.md) | [03 - AWS CodeDeploy](./03-AWS-CodeDeploy-AppSpec-and-Deployment-Strategies.md) |
