# 03 - AWS CodeDeploy & Deployment Strategies

- **In-Place Deployment**: Updates instances sequentially (reduces capacity during deployment).
- **Blue/Green Deployment**: Provisions new instances/tasks with updated code, routes traffic over ALB, and terminates old instances upon verification.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - AWS CodeBuild](./02-AWS-CodeBuild-buildspec-yml-and-Build-Environments.md) | [README](./README.md) | [04 - AWS CodePipeline](./04-AWS-CodePipeline-Orchestration-and-Stage-Artifacts.md) |
