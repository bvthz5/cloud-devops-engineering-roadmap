# 15 — Kubernetes Pod CrashLoopBackOff Troubleshooting

## 1. Scenario
A Kubernetes pod enters `CrashLoopBackOff` status after a new container image deployment.

```text
Scenario
   ↓
Symptoms: Pod status CrashLoopBackOff, Restart count continuously increments
   ↓
What could cause it? Application crash on boot, failing liveness probe, missing secret / env var
   ↓
Diagnostic commands: kubectl get pods, kubectl describe pod pod_name, kubectl logs pod_name --previous
```

## 2. Step-by-Step Diagnostic Workflow

```bash
# Step 1: Check pod status and restart count
kubectl get pods -n production

# Step 2: Inspect events and container status (exit code, reason)
kubectl describe pod web-app-7f58d9-x9z21 -n production

# Step 3: Fetch logs from CURRENT crashed container instance
kubectl logs web-app-7f58d9-x9z21 -c web-container -n production

# Step 4: Fetch logs from PREVIOUS container instance before crash
kubectl logs web-app-7f58d9-x9z21 -c web-container --previous -n production
```

## 3. Key Pod Failure States Reference

| Pod Status | Primary Cause | Fix |
|---|---|---|
| `CrashLoopBackOff` | App process crashes after start. | Check `kubectl logs --previous`. Fix app code / config. |
| `ImagePullBackOff` | Image tag missing, wrong registry URL, or bad imagePullSecrets. | Verify image name, tag, and docker credentials. |
| `OOMKilled` | Container exceeded `resources.limits.memory`. | Increase pod memory limit in Deployment manifest. |
| `Pending` | Insufficient CPU/Memory capacity in cluster, or nodeSelector mismatch. | Check `kubectl describe pod` Events for scheduler errors. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Docker Troubleshooting](./14-Docker-Container-Troubleshooting.md) | [README](./README.md) | [16 - Suspicious Processes & Security Incidents](./16-Suspicious-Processes-and-Security-Incident-Response.md) |
