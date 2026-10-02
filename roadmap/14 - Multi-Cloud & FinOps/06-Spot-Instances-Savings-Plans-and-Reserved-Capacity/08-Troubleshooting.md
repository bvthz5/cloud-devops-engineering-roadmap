# Troubleshooting Guide - Spot Instances, Savings Plans & Reserved Capacity

> **Module**: Spot Instances, Savings Plans & Reserved Capacity

---

## 🔍 Common Issue 1: Infracost CI/CD Pull Request Pipeline Failure

### Symptom
The GitHub Actions workflow fails on the `infracost diff` step with `Error: Invalid API Key` or `Error parsing HCL modules`.

### Root Cause & Resolution
1. **Missing API Key**: Ensure `INFRACOST_API_KEY` is added to GitHub Repository Secrets.
2. **Private Module Access**: If Terraform uses private Git modules, ensure the SSH agent step or `GIT_SSH_COMMAND` is configured prior to running Infracost.
3. **Local Cache Clearance**: Run `infracost clean` locally to reset provider schema caches.

---

## 🔍 Common Issue 2: Unexpected Spot Instance Eviction & Job Failures

### Symptom
Batch processing jobs fail mid-execution due to two-minute Spot instance termination notifications.

### Root Cause & Resolution
1. **Lack of Graceful Shutdown**: Worker pods are killed abruptly when nodes terminate.
2. **Spot Interruption Handler**: Deploy `aws-node-termination-handler` or GCP Spot eviction listeners to catch termination signals 120 seconds in advance.
3. **Checkpointing**: Implement checkpointing in batch code so interrupted workers resume processing from the last saved state on replacement nodes.
