# 01 - Kubernetes Job Controller and Completion Guarantees

## 1. What Is a Kubernetes Job?

While standard Kubernetes controllers like `Deployments` and `StatefulSets` are designed to keep long-running background processes (such as HTTP APIs, web servers, and message brokers) running indefinitely, a **Job** is designed for **batch workloads that run to completion**.

A Job supervisor starts one or more Pods, tracks their exit statuses, and guarantees that a specified number of Pods terminate successfully with **Linux Exit Code 0**. Once the required number of pods complete, the Job enters a `Completed` state and stops creating new pods. The successfully completed pods are retained in `Completed` state so that operators can inspect their stdout/stderr logs and exit codes.

### Primary Use Cases:
- **Database Schema Migrations:** Executing Flyway, Liquibase, or Django/Rails migration scripts before rolling out an application update.
- **Batch Data Pipelines:** Running Nightly ETL jobs, Apache Spark batch tasks, or CSV ingestion pipelines.
- **Machine Learning Offline Processing:** Feature extraction, model evaluation, and dataset preprocessing.
- **Disaster Recovery Tasks:** Exporting database snapshots to Amazon S3 or Google Cloud Storage.

```text
+---------------------------------------------------------------------------------------------------+
|                        DEPLOYMENT (DAEMON) VS JOB (RUN-TO-COMPLETION)                             |
+---------------------------------------------------------------------------------------------------+

 DEPLOYMENT: CONTINUOUS PROCESS (Exit Code 0 is treated as an unexpected failure!)
 ┌────────────────────────────────────────────────────────┐
 │ Container executes command ──► Exits 0                 │
 │ Kubelet Intervention: "Process died! Restarting pod!"  │ (Restarts in a loop forever!)
 └────────────────────────────────────────────────────────┘

 JOB: BATCH RUN-TO-COMPLETION (Exit Code 0 represents SUCCESS)
 ┌────────────────────────────────────────────────────────┐
 │ Container executes script ──► Exits 0                  │
 │ Job Controller: "Task succeeded! Mark Pod Completed!"  │ (Pod stays dead; no restarts!)
 └────────────────────────────────────────────────────────┘
```

---

## 2. The Strict `restartPolicy` Constraint

A fundamental architectural rule of the Job specification is that **`spec.template.spec.restartPolicy` cannot be set to `Always`**. If an engineer submits a Job with `restartPolicy: Always`, the `kube-apiserver` schema validation webhook rejects the manifest immediately.

Jobs permit only two restart policies:
1. **`restartPolicy: OnFailure`:** If the container crashes with a non-zero exit code, the local node `kubelet` restarts the container inside the **same physical Pod** without rescheduling.
2. **`restartPolicy: Never`:** If the container fails, the Kubelet leaves the Pod in `Failed` state. The Job controller notices the failure and schedules a **completely brand-new Pod** onto another worker node.

---

## 3. The 4 Job Completion Patterns

The Job controller supports four distinct execution topologies governed by `completions`, `parallelism`, and `completionMode`:

```text
+---------------------------------------------------------------------------------------------------+
|                           THE 4 JOB COMPLETION TOPOLOGIES                                         |
+---------------------------------------------------------------------------------------------------+

 1. Non-Parallel Single Job:
    completions: 1, parallelism: 1
    Launches exactly 1 pod. Job completes when that single pod exits 0.

 2. Fixed Completion Parallel Job:
    completions: 10, parallelism: 3
    Job maintains up to 3 concurrently running pods. As pods exit 0, new ones spawn
    until a total of 10 successful completions are achieved.

 3. Work Queue Parallel Job:
    completions: nil, parallelism: 5
    Launches 5 concurrent worker pods. Pods coordinate independently by pulling items from
    an external RabbitMQ or SQS queue. When the queue is empty, workers exit 0.

 4. Indexed Job (completionMode: Indexed):
    completions: 4, parallelism: 2
    Kubernetes assigns a unique deterministic index (0, 1, 2, 3) to each pod via the
    JOB_COMPLETION_INDEX environment variable. Perfect for distributed ML data partitions!
```

---

## 4. Production Manifest: Database Migration Job with Automated TTL Cleanup

The following manifest demonstrates a production database schema migration job configured with exponential backoff, hard deadlines, and automatic garbage collection:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: flyway-schema-migration-v2
  namespace: production
  labels:
    app.kubernetes.io/component: migration
spec:
  # Total successful completions required
  completions: 1
  parallelism: 1
  # Maximum failed retries before marking Job as failed (default: 6)
  backoffLimit: 3
  # Hard timeout: if job runs longer than 15 minutes, abort and kill pods!
  activeDeadlineSeconds: 900
  # Automatically delete the Job and its pods 10 minutes after completion
  ttlSecondsAfterFinished: 600
  template:
    metadata:
      labels:
        app.kubernetes.io/component: migration
    spec:
      restartPolicy: OnFailure
      containers:
        - name: schema-migration
          image: flyway/flyway:10.7-alpine
          args:
            - "-url=jdbc:postgresql://postgres.database.svc:5432/finance"
            - "-user=postgres"
            - "migrate"
          env:
            - name: FLYWAY_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-credentials
                  key: postgres-password
          resources:
            requests:
              cpu: 200m
              memory: 256Mi
            limits:
              cpu: 1000m
              memory: 512Mi
```

---

## 5. Failure Handling: Backoff Limits and Pod Failure Policies

When a Job pod fails with a non-zero exit code:
1. The Job controller re-runs the pod using an **exponential backoff delay** ($10\text{s}, 20\text{s}, 40\text{s}, \dots$ capped at 6 minutes).
2. The number of failed attempts is tracked in `status.failed`. When `failed >= backoffLimit`, the controller terminates all active pods and marks the Job condition as `Failed`.

### Pod Failure Policy (Granular Exit Code Rules)
In modern Kubernetes (v1.25+), operators can define a `podFailurePolicy` to differentiate between transient network glitches (which should be retried) and fatal programming errors (which should fail immediately without wasting compute cycles):

```yaml
spec:
  backoffLimit: 6
  podFailurePolicy:
    rules:
      # If container exits with 42 (Fatal Configuration Error), fail job immediately!
      - action: FailJob
        onExitCodes:
          containerName: schema-migration
          operator: In
          values: [42]
      # If container exits due to host SIGKILL (OOM 137), retry count is ignored
      - action: Ignore
        onPodConditions:
          - type: DisruptionTarget
```

---

## 6. Operational Debugging Commands

```bash
# Check status of jobs (COMPLETIONS, DURATION, AGE)
kubectl get jobs -n production

# Inspect detailed failure reasons, backoff count, and pod events
kubectl describe job flyway-schema-migration-v2 -n production

# View logs of the completed or failed migration pod
kubectl logs -n production -l job-name=flyway-schema-migration-v2

# Manually clean up old finished jobs that lacked ttlSecondsAfterFinished
kubectl delete jobs --field-selector status.successful=1 -n production
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-StatefulSets-and-DaemonSets)](../09-StatefulSets-and-DaemonSets/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Parallel Jobs and Work Queue Processing →](./02-Parallel-Jobs-and-Work-Queue-Processing.md) |
