# 02 - AWS CodeBuild: Build Environments & buildspec.yml Lifecycle

> **Focus:** Fully managed continuous integration build service, Docker-based build containers, and the 4 phases of a `buildspec.yml` file.

---

## 🛠️ 1. Production buildspec.yml Structure

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.11
    commands:
      - echo "Installing runtime dependencies..."
      - pip install -r requirements.txt
  pre_build:
    commands:
      - echo "Logging into Amazon ECR..."
      - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
      - REPO_URI=123456789012.dkr.ecr.us-east-1.amazonaws.com/api-service
      - COMMIT_HASH=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
  build:
    commands:
      - echo "Building Docker image on $(date)..."
      - docker build -t $REPO_URI:latest .
      - docker tag $REPO_URI:latest $REPO_URI:$COMMIT_HASH
  post_build:
    commands:
      - echo "Pushing Docker image to ECR..."
      - docker push $REPO_URI:latest
      - docker push $REPO_URI:$COMMIT_HASH
      - printf '[{"name":"api-service","imageUri":"%s"}]' $REPO_URI:$COMMIT_HASH > imagedefinitions.json

artifacts:
  files:
    - imagedefinitions.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AWS DevOps Services Overview](./01-AWS-DevOps-Services-Overview.md) | [Index](../../../README.md) | [03 - AWS CodeDeploy AppSpec and Deployment Strategies →](./03-AWS-CodeDeploy-AppSpec-and-Deployment-Strategies.md) |
