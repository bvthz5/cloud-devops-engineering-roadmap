# CI/CD Pipelines & Automation Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: GitHub Actions Self-Hosted Runners Swamped by Job Concurrency Bottlenecks

### 🚨 The Production Scenario
During release week, pull request checks in GitHub Actions queue for over 2 hours. Self-hosted runner VMs run at 100% CPU and run out of disk space, failing builds with 'No space left on device'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Runners were statically provisioned without ephemeral auto-scaling. Builds did not clean up Docker volumes and intermediate build caches, causing disk space leakage. High job concurrency on static runners starved disk I/O.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy Actions Runner Controller (ARC) on Kubernetes to dynamically scale ephemeral runners based on workflow queue demand.
- Configure Docker-in-Docker (dind) or container-mode runners with isolated ephemeral storage per build job.
- Implement GitHub Actions cache action with cache retention limits and remote object storage backends.
- Add post-job cleanup hooks to prune Docker images and dangling build artifacts.
- Set workflow timeouts and concurrency groups with 'cancel-in-progress: true' on pull request pipelines.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect ARC RunnerDeployment status and replica count
kubectl get runnerdeployments -n arc-systems

# Check autoscaling metrics and min/max runner limits
kubectl get horizontalrunnerautoscalers -n arc-systems -o yaml

# Emergency cleanup of runner host Docker storage
docker system df && docker system prune -af --volumes

# List queued GitHub Actions workflow runs via CLI
gh run list --status queued

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Static CI/CD runners inevitably bottleneck during peak sprint cycles. I migrate static runners to Actions Runner Controller (ARC) on Kubernetes. ARC listens to GitHub webhook events and spins up isolated, single-use ephemeral runner pods in seconds that terminate immediately after job completion, eliminating disk pollution and job starvation."

---

## 📌 Scenario 2: Jenkins Zombie Builds & Master Controller Heap OOM Crash

### 🚨 The Production Scenario
Jenkins master controller crashes repeatedly with 'java.lang.OutOfMemoryError: Java heap space'. Jenkins UI becomes completely unresponsive, and dozens of orphaned pipeline jobs run indefinitely.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Jenkins was running heavy build workloads and pipeline scripts directly on the controller node rather than distributed agents. Massive build histories, excessive workspace retention, and thread leaks from unclosed pipeline steps exhausted the JVM heap.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Increase JVM heap parameters (-Xms, -Xmx) and configure G1GC garbage collection.
- Configure Jenkins Kubernetes Plugin to launch dynamic, ephemeral pod agents for every job execution.
- Enforce Jenkins global configuration: 'Restrict where this project can be run' with 0 executors on controller.
- Configure 'Discard old builds' policy globally to limit log retention to 30 builds or 14 days.
- Install and configure Jenkins Support Core plugin to capture JVM thread dumps and heap dumps during incidents.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect live Jenkins JVM heap memory distribution
jcmd <JENKINS_PID> GC.heap_info

# Generate thread dump to analyze lock contention and zombie threads
jcmd <JENKINS_PID> Thread.print > /tmp/jenkins_threads.tdump

# Trigger graceful Jenkins controller restart
curl -X POST http://jenkins.internal/safeRestart --user admin:api_token

# Inspect dynamic Kubernetes build agent pods
kubectl get pods -n jenkins-agents -l jenkins=agent

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A Jenkins controller should act strictly as an orchestrator, never an execution engine. I enforce 0 executors on the master node, offloading 100% of pipeline jobs to ephemeral Kubernetes agent pods. I tune JVM heap with G1GC, implement automated build history rotation, and set global pipeline timeouts to eliminate zombie executions."

---

## 📌 Scenario 3: GitLab CI Pipeline Failure Due to Secret Masking and Regex Collision

### 🚨 The Production Scenario
A production deployment job in GitLab CI abruptly fails with: 'This job failed because the input string matched a masked variable pattern'. The build logs are heavily corrupted with '[MASKED]'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A masked secret variable in GitLab CI was set to a common short string (e.g., '123' or 'test'), causing GitLab CI log sanitizer to aggressively mask matching substrings in legitimate compilation commands, corrupting base64 tokens and artifact hashes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Audit project and group CI/CD variables in GitLab to identify short or poorly generated secret tokens.
- Enforce secret complexity standards: all masked variables must have high entropy and minimum 8 characters.
- Use external secret managers (HashiCorp Vault or AWS Secrets Manager) via GitLab ID Tokens (OIDC) rather than static project variables.
- Rerun the job after correcting the variable value to unmask pipeline logs.
- Sanitize build log outputs to prevent secret leakage in build artifacts.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect GitLab CI variables and masked status via CLI
glab variable list --repo owner/repo

# Fetch raw job execution trace logs
glab ci trace <JOB_ID>

# Retry pipeline job after variable correction
glab ci retry <JOB_ID>

# Verify Vault secret entropy and rotation status
vault read secret/data/production/api-keys

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "GitLab CI regex-masks secret strings across all console output. If an engineer sets a masked variable to a common string or short hash, the log scrubber redacts valid commands and breaks pipeline parsers. I audit CI/CD variables to ensure masked secrets meet strict entropy thresholds and migrate production credentials to Vault using dynamic OIDC authentication."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Google Cloud Platform (GCP) Infrastructure Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../10-Google-Cloud-Platform-GCP-Infrastructure-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [CI/CD Pipelines & Automation Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

