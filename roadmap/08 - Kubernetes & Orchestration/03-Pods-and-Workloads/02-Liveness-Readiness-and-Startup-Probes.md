# 02 - Liveness, Readiness, and Startup Probes

## 1. The Three Health Probes

```text
        STARTUP PROBE
        Is application finished bootstrapping?
        ├── NO  ──► Wait (or Kill if failureThreshold exceeded)
        └── YES ──► Enable Liveness & Readiness Probes
                         │
                         ▼
        READINESS PROBE                          LIVENESS PROBE
        Can app accept incoming traffic?         Is app running or deadlocked?
        ├── NO  ──► Remove from Service endpoints ├── NO  ──► KILL & RESTART CONTAINER
        └── YES ──► Route traffic to this Pod    └── YES ──► Leave container running
```

---

## 2. Production Probe Specification

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: robust-api
spec:
  containers:
  - name: api
    image: my-org/api:v1.2.0
    ports:
    - containerPort: 8080
    
    # 1. Startup Probe: Protects slow-starting JVM/Node/Python apps
    startupProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
      failureThreshold: 24    # Gives up to 120 seconds to boot
      
    # 2. Liveness Probe: Detects deadlocks and fatal freezes
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      periodSeconds: 10
      timeoutSeconds: 3
      failureThreshold: 3
      
    # 3. Readiness Probe: Manages traffic routing
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      periodSeconds: 5
      timeoutSeconds: 2
      successThreshold: 1
      failureThreshold: 2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Pod Architecture](./01-Pod-Architecture-Pause-Container-and-Lifecycle.md) | [README](./README.md) | [03 - Init & Sidecars](./03-Init-Containers-Sidecars-and-Ephemeral-Containers.md) |
