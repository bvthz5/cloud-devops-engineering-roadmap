# 10 - Hands-On Practice Labs

## Lab 1: Simulating and Diagnosing an OOMKill Event

### Objective
Launch a container with strict memory limits, trigger an intentional memory allocation breach, and verify the OOM exit code.

### Implementation
```bash
# 1. Run container with 50MB memory limit that allocates 100MB RAM
docker run --name oom_test -m 50m python:alpine python -c "x = ' ' * (100 * 1024 * 1024)"

# 2. Inspect exit code
echo "Exit Code: $(docker inspect --format '{{.State.ExitCode}}' oom_test)"
# Output: Exit Code: 137

# 3. Verify OOM flag
docker inspect --format 'OOMKilled: {{.State.OOMKilled}}' oom_test
# Output: OOMKilled: true
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
