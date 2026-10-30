# 01 - Pod Failure States: CrashLoopBackOff, OOMKilled, and ImagePullBackOff

## 1. The Production SRE Pod Debugging Decision Tree

When an application Pod fails in a production Kubernetes cluster, Site Reliability Engineers (SREs) follow a systematic triage workflow to isolate the root cause within seconds:

```text
+---------------------------------------------------------------------------------------------------+
|                        THE PRODUCTION POD TRIAGE DECISION TREE                                    |
+---------------------------------------------------------------------------------------------------+

                                    [ Pod is Unhealthy ]
                                             │
                        ┌────────────────────┴────────────────────┐
                        ▼                                         ▼
                STATUS: "Pending"                       STATUS: Not "Pending"
                        │                                         │
        ┌───────────────┴───────────────┐                         ▼
        ▼                               ▼                 Inspect Pod Events:
 Insufficient CPU/RAM?           Volume Stuck?         kubectl describe pod <name>
 (FailedScheduling event)   (FailedMount / PVC error)             │
                                        ┌─────────────────────────┼─────────────────────────┐
                                        ▼                         ▼                         ▼
                              ImagePullBackOff?             OOMKilled?              CrashLoopBackOff?
                                        │                         │                         │
                                        ▼                         ▼                         ▼
                              - Typo in tag?            - Exit Code: 137?         - Run: kubectl logs
                              - Missing secret?         - cgroup memory limit       <name> --previous
                              - Docker rate limit?        exceeded?               - App syntax crash?
                              - Registry blocked?       - Java heap misconfig?    - Missing DB conn?
```

---

## 2. Failure State 1: `CrashLoopBackOff`

### What It Means:
`CrashLoopBackOff` indicates that the container successfully started, but the process inside the container terminated unexpectedly (usually with a non-zero exit code). Because the Pod's `restartPolicy` is set to `Always` or `OnFailure`, the local node `kubelet` attempts to revive the process. To prevent a crashing application from overwhelming the host CPU with continuous reboot loops, the Kubelet introduces an **exponential backoff delay** ($10\text{s}, 20\text{s}, 40\text{s}, 80\text{s}, 160\text{s}$, capped at $300\text{s}$).

### The #1 SRE Debugging Trap:
Running `kubectl logs <pod-name>` often outputs **nothing** because the container is currently in the delay period or just rebooted into an empty stdout stream.
You must use the **`--previous`** flag to inspect the log output of the *terminated container instance*:

```bash
kubectl logs <pod-name> -n <namespace> --previous
```

### Common Root Causes:
1. **Missing Required Environment Variables:** The application expects `DATABASE_URL` or `API_KEY` to be set; when absent, the code panics and calls `os.Exit(1)`.
2. **Database or Network Connection Timeouts:** The application attempts to connect to a PostgreSQL database on startup. If the database service is down or DNS is failing, the connection timeout triggers a fatal exception.
3. **Improper Entrypoint or Command:** The Dockerfile specifies an invalid command or script that does not possess executable permissions (`chmod +x entrypoint.sh`).
4. **Daemon Exiting Immediately (Exit Code 0):** Running a script that finishes in 2 seconds inside a Deployment. Deployments require long-running foreground processes; if the command finishes, Kubernetes assumes the server crashed and restarts it!

---

## 3. Failure State 2: `OOMKilled` (Linux Exit Code 137)

### What It Means:
`OOMKilled` stands for **Out Of Memory Killed**. It occurs when a container process attempts to allocate more physical RAM than allowed by its cgroup memory limit (`spec.containers[*].resources.limits.memory`).

The Linux kernel Out-Of-Memory subsystem triggers and sends an uncatchable **`SIGKILL` (Signal 9)** to the offending process:

$$\text{Linux Exit Code} = 128 + \text{Signal Number} = 128 + 9 = 137$$

```text
+---------------------------------------------------------------------------------------------------+
|                        HOW THE LINUX KERNEL ENFORCES OOM KILL (EXIT 137)                          |
+---------------------------------------------------------------------------------------------------+

 1. Container Limit Declared: memory: 512Mi
 ┌────────────────────────────────────────────────────────┐
 │ Linux cgroup controller: memory.max = 536870912 bytes  │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 2. Application Process allocates memory:
    Process requests 513MB from kernel via malloc() / brk()
                             │
                             ▼
 3. Kernel cgroup memory subsystem breaches threshold:
    Action: Kernel invokes oom-killer routine.
    Signal: Sends SIGKILL (Signal 9) directly to process PID.
                             │
                             ▼
 4. Process instantly terminated: Container exits with Exit Code 137.
    Kubelet records status: Terminated, Reason: OOMKilled.
```

### Diagnostic Command:
```bash
kubectl describe pod <pod-name> | grep -E "(Last State|Reason|Exit Code)"
```
*Output confirming OOM:*
```text
    Last State:     Terminated
      Reason:       OOMKilled
      Exit Code:    137
```

### Remediation Strategies:
1. Increase `resources.limits.memory` in the Deployment manifest.
2. For Java / JVM runtimes, ensure heap boundaries are configured correctly (`-XX:MaxRAMPercentage=75.0` or `-Xmx`) to prevent the JVM from allocating heap beyond the container cgroup boundary.
3. Inspect memory leaks using profiling tools (pprof, heap dumps).

---

## 4. Failure State 3: `ImagePullBackOff` and `ErrImagePull`

### What It Means:
The node `kubelet` was instructed to launch a Pod, but the container runtime (`containerd` / `CRI-O`) was unable to successfully download the container image layers from the container image registry.

### Diagnostic Command:
Always inspect the **Events** section at the bottom of `kubectl describe`:

```bash
kubectl describe pod <pod-name>
```

```text
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  2m                 default-scheduler  Successfully assigned default/web-app to node-01
  Normal   Pulling    1m (x3 over 2m)    kubelet            Pulling image "private-registry.com/app:v1.2.0"
  Warning  Failed     1m (x3 over 2m)    kubelet            Failed to pull image: rpc error: code = Unknown desc = failed to pull and unpack image: failed to resolve reference: unexpected status code 401 Unauthorized
  Warning  Failed     1m (x3 over 2m)    kubelet            Error: ErrImagePull
  Normal   BackOff    45s (x4 over 2m)   kubelet            Back-off pulling image "private-registry.com/app:v1.2.0"
  Warning  Failed     45s (x4 over 2m)   kubelet            Error: ImagePullBackOff
```

### Root Causes and Resolutions:

| Error Substring in Event | Underlying Root Cause | Corrective Action |
| :--- | :--- | :--- |
| `status code 401 Unauthorized` | Missing or invalid registry authentication credentials. | Create a `kubernetes.io/dockerconfigjson` Secret and reference it under `imagePullSecrets`. |
| `manifest unknown / not found` | Image repository name is misspelled or the image tag does not exist. | Verify spelling; verify image was pushed to registry using `docker images` or registry UI. |
| `status code 429 Too Many Requests`| Docker Hub anonymous rate limiting exceeded (100 pulls per 6 hours). | Configure enterprise authenticated pull secrets or mirror images to AWS ECR / GCP Artifact Registry. |
| `dial tcp: i/o timeout` | Worker node cannot reach the registry due to network firewall / security groups. | Verify outbound internet routing, NAT gateway, and DNS configuration on worker node. |

---

## 5. Failure State 4: `CreateContainerConfigError`

### What It Means:
The container image was pulled successfully, but the Kubelet cannot initialize the container runtime configuration.

### Root Causes:
1. **Missing ConfigMap or Secret:** The container manifest references a `ConfigMapKeyRef` or `SecretKeyRef` that does not exist in the namespace:
   ```yaml
   env:
     - name: DB_PASSWORD
       valueFrom:
         secretKeyRef:
           name: non-existent-secret # Pod will fail with CreateContainerConfigError!
           key: password
   ```
2. **Missing `optional: true` flag:** If an environment variable references an optional configmap, set `optional: true` to prevent container creation failures when the config is absent.

---

## 6. Comprehensive Pod Status Quick Reference

```text
+----------------------------+---------------------------------------------------+---------------------------------------------------+
| Pod Status Code            | Core Problem                                      | Primary Remediation Step                          |
+----------------------------+---------------------------------------------------+---------------------------------------------------+
| CrashLoopBackOff           | Application runtime panic or early termination    | Check logs: kubectl logs <pod> --previous         |
| OOMKilled (Exit 137)       | Memory limit breached; kernel sent SIGKILL        | Increase memory limits; fix application leak      |
| ImagePullBackOff           | Registry authentication or image tag failure      | Check imagePullSecrets and tag spelling           |
| CreateContainerConfigError | Missing Secret or ConfigMap reference             | Verify referenced Secret/ConfigMap exists         |
| Pending                    | Insufficient node resources or unscheduled PVC    | Check events: kubectl describe pod <pod>          |
| Terminating (Stuck)        | Finalizer blocked or dead node unmount failure    | Inspect finalizers: kubectl get pod -o yaml       |
+----------------------------+---------------------------------------------------+---------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (15-CRDs-and-Kubernetes-Operators)](../15-CRDs-and-Kubernetes-Operators/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Node NotReady Troubleshooting and Kubelet Diagnostics →](./02-Node-NotReady-Troubleshooting-and-Kubelet-Diagnostics.md) |
