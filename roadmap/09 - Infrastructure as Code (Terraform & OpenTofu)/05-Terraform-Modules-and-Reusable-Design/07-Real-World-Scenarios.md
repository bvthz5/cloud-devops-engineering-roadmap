# 07 - Real-World Scenarios

## Scenario 01: Enterprise Module Library

### Challenge
A company with 50 teams needed consistent AWS infrastructure across all projects.

### Solution
1. Created an internal module registry using private GitHub repos
2. Built opinionated modules for VPC, EKS, RDS, and S3 with security defaults baked in
3. Enforced module usage via Sentinel policies (no raw `aws_vpc` resources allowed)
4. Published changelog and migration guides for every module version bump

### Result
Reduced new project setup from 2 weeks to 2 hours.

---

## Scenario 02: Module Version Pinning Failure

### Incident
A team used `source = "terraform-aws-modules/vpc/aws"` without a `version` constraint. A new major version was released overnight, breaking their CI pipeline.

### Resolution
Pinned all modules with `version = "~> 5.0"` and added CI checks that reject unpinned module references.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Module Testing with Terratest and terraform test](./06-Module-Testing-with-Terratest-and-terraform-test.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
