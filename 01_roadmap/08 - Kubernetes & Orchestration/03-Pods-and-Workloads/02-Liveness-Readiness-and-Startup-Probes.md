# 02 - Liveness, Readiness, and Startup Probes

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Kubernetes health probes, contrasting Startup, Liveness, and Readiness probes, exploring probe mechanisms (HTTP, TCP, Exec, gRPC), avoiding cascading failure antipatterns, and production tuning.

---

## 🏛️ 1. What Are Health Probes & Why Does Process Monitoring Fail?

In traditional operating systems, administrators determine whether a program is functioning normally by checking whether its process identifier (PID) exists in the process table.

In modern distributed microservices, **process presence is an unreliable indicator of application health**:
- An application process may be alive on the CPU, but trapped in an unrecoverable **deadlock** between two database threads.
- A service may have exhausted its database connection pool, responding to all incoming user requests with `HTTP 500 Internal Server Error`.
- A Java Spring Boot or machine learning service may require 90 seconds to load models and initialize caches into memory, during which it cannot accept user traffic.

If an orchestrator only checked whether PID 1 was alive, it would blindly route production client traffic to deadlocked or booting containers, causing widespread outages.

**Kubernetes Health Probes** solve this challenge by allowing the `kubelet` on the local worker node to periodically query the application internally through structured health probes, taking automated corrective actions based on the response.

---

## ⚙️ 2. The Three Distinct Kubernetes Health Probes

Kubernetes provides three specialized health probes that govern different phases of a container's operational lifecycle:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE THREE HEALTH PROBES IN ACTION                    │
└────────────────────────────────────────────────────────────────────────┘

 Container Initialized / Booting
         │
         ▼
 [ 1. STARTUP PROBE ]
   "Has the application finished bootstrapping?"
   ├── NO  ──► Wait (disables Liveness & Readiness probes during boot)
   │           If failureThreshold exceeded ──► Kubelet kills & restarts container!
   └── YES ──► Permanently disables itself! Hands off to Liveness & Readiness!
                    │
                    ├─────────────────────────────────────────┐
                    ▼                                         ▼
 [ 2. READINESS PROBE ]                    [ 3. LIVENESS PROBE ]
   "Can this Pod accept traffic right now?"  "Is this application alive or deadlocked?"
   ├── NO  ──► Remove Pod IP from Services!  ├── NO  ──► KILL & RESTART CONTAINER!
   │           Zero traffic routed to Pod!   │           Invokes restartPolicy!
   └── YES ──► Add Pod IP to EndpointSlices! └── YES ──► Container continues running.
               Direct client traffic here.
```

### 2.1 The Startup Probe: Protecting Slow Bootstraps
- **Why it was invented:** Historically, developers configured high `initialDelaySeconds` on Liveness probes to give heavy applications (like JVM or Python Django apps) time to start. However, if an app took 60 seconds to boot on average but occasionally took 90 seconds under high node load, the Liveness probe would prematurely kill the container, trapping it in an endless restart loop!
- **Working Mechanism:** The **Startup Probe** executes exclusively during initial container startup. **It completely disables Liveness and Readiness probes** until it succeeds. Once the Startup probe passes for the first time, it shuts down permanently, handing operational monitoring over to the Liveness and Readiness probes.

### 2.2 The Liveness Probe: Healing Deadlocks and Freezes
- **Purpose:** Answers the question: *Is the application process in a healthy, recoverable state?*
- **Action on Failure:** If the Liveness probe fails for `failureThreshold` consecutive attempts, the `kubelet` **kills the container and restarts it** according to its configured `restartPolicy`.
- **Primary Use Case:** Recovering from unhandled runtime exceptions, memory leaks that freeze event loops, or multi-threaded deadlocks that the application cannot resolve internally.

### 2.3 The Readiness Probe: Regulating Network Traffic
- **Purpose:** Answers the question: *Is the application currently ready to accept incoming network requests?*
- **Action on Failure:** **The kubelet DOES NOT kill or restart the container.** Instead, it notifies the API server, which immediately removes the Pod's private IP address from all matching Service **`EndpointSlices`**.
- **Primary Use Case:** Taking an overloaded pod temporarily out of service while it processes a heavy batch calculation, re-indexes a cache, or performs warm-up tasks, ensuring client requests are routed only to sibling pods that are ready.

---

## ⚠️ 3. The Dangerous Liveness Dependency Cascading Failure Antipattern

The single most common and destructive mistake in Kubernetes probe design is **checking external downstream dependencies (such as databases or third-party APIs) inside a Liveness probe**:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE CASCADING OUTAGE ANTIPATTERN                     │
└────────────────────────────────────────────────────────────────────────┘

 1. Downstream Database encounters a transient 2-second network blip.
         │
         ▼
 2. API Pods execute Liveness Probe: "Can I query the database?"
    Database fails to respond within timeout!
         │
         ▼
 3. Kubelet interprets database failure as APPLICATION CORRUPTION!
    Kubelet simultaneously KILLS and RESTARTS all 50 API Pods!
         │
         ▼
 4. All 50 API Pods reboot simultaneously, flooding database with 50 cold
    connection handshakes, permanently crashing the recovering database!
    💥 TOTAL CASCADING CLUSTER OUTAGE!
```

### The Golden Rule of Probes:
- **Liveness Probes must be strictly localized:** Test *only* internal application health (e.g., "Is my local HTTP server responding on localhost? Are my internal worker threads running?"). Never check external databases, Redis caches, or external APIs in a Liveness probe!
- **Readiness Probes may check dependencies:** If a database is unreachable, a Readiness probe *should* fail so that the pod stops receiving web traffic, but the pod remains running quietly until the database recovers.

---

## 🔬 4. The Four Probe Mechanisms

Kubernetes supports four distinct mechanisms to execute health checks:

| Mechanism | Configuration Directive | Working Mechanism | Best Used For |
| :--- | :--- | :--- | :--- |
| **HTTP GET** | `httpGet:` | Kubelet sends an HTTP `GET` request to a declared path and port. Any HTTP status code between **200 and 399** is evaluated as healthy. | Web applications, REST APIs, and microservices. |
| **TCP Socket** | `tcpSocket:` | Kubelet attempts to establish a raw TCP connection to the specified port. If the socket connects successfully, the probe passes. | Daemons without HTTP interfaces (databases, Redis, Kafka, FTP). |
| **Exec Command** | `exec:` | Kubelet spawns a diagnostic process inside the container. If the process exits with **Exit Code 0**, the probe passes; any non-zero exit code indicates failure. | Custom shell scripts, checking file markers or local socket health. |
| **gRPC Native** | `grpc:` | Kubelet invokes the standardized **gRPC Health Checking Protocol** over an active gRPC connection. (Introduced in K8s v1.24). | Native gRPC backends and microservice RPC meshes. |

---

## 📝 5. Production-Grade Complete Probe Specification

The following YAML manifest demonstrates an enterprise-grade configuration combining Startup, Liveness, and Readiness probes with tuned timing parameters:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: enterprise-api-service
  labels:
    app.kubernetes.io/name: backend-api
spec:
  containers:
  - name: api
    image: mycompany/production-api:v2.4.0
    ports:
    - containerPort: 8080
      name: http-traffic

    # 1. Startup Probe: Grants up to 120 seconds for slow application bootstrapping
    startupProbe:
      httpGet:
        path: /healthz/startup
        port: http-traffic
      initialDelaySeconds: 5     # Wait 5s before first probe
      periodSeconds: 5           # Probe every 5s thereafter
      timeoutSeconds: 2          # Timeout after 2s
      failureThreshold: 24       # 24 failures * 5s = 120s maximum boot window!

    # 2. Liveness Probe: Detects catastrophic application thread deadlocks
    livenessProbe:
      httpGet:
        path: /healthz/liveness
        port: http-traffic
      periodSeconds: 10          # Check health every 10s
      timeoutSeconds: 3          # Strict 3s timeout
      failureThreshold: 3        # Restarts container after 3 consecutive failures

    # 3. Readiness Probe: Manages traffic routing without restarting
    readinessProbe:
      httpGet:
        path: /healthz/readiness
        port: http-traffic
      periodSeconds: 5           # Check readiness frequently
      timeoutSeconds: 2          # Strict timeout
      successThreshold: 1        # Restores to Service immediately on 1 success
      failureThreshold: 2        # Removes from Service after 2 consecutive failures
```

---

## 📋 6. Summary and Navigation

Health probes provide the automated feedback loops required for true cloud-native self-healing. By utilizing Startup probes to protect slow application initialization, Liveness probes to recover from internal deadlocks, and Readiness probes to govern traffic routing, engineering teams eliminate downtime and prevent cascading outages.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Pod Architecture Pause Container and Lifecycle](./01-Pod-Architecture-Pause-Container-and-Lifecycle.md) | [Index](../../../README.md) | [03 - Init Containers Sidecars and Ephemeral Containers →](./03-Init-Containers-Sidecars-and-Ephemeral-Containers.md) |
