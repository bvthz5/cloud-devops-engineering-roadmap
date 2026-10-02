# ☸️ Kubernetes (K8s) — Complete Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, advantages, real-life analogies, and command breakdown. Covers every Kubernetes component, resource, probe, and kubectl command.

---

## 🧠 1. What is Kubernetes? (1-Line Definition)

- **Kubernetes (K8s):** An open-source orchestrator that automates deploying, scaling, healing, and managing thousands of containerized applications across a cluster of servers.
- **Real-Life Analogy:** An automated shipping port crane operator that unloads, inspects, and organizes shipping containers onto available trucks.
- **Main Advantage:** If an application crashes or a server catches fire, Kubernetes restarts or relocates containers instantly without downtime.

---

## 🏗️ 2. Kubernetes Architecture: Control Plane vs Worker Nodes

A Kubernetes cluster is divided into two distinct parts:

```text
┌─────────────────────────────────────────────────────────────┐
│                 CONTROL PLANE (The Brain)                   │
│                                                             │
│   ┌──────────────────┐               ┌──────────────────┐   │
│   │  kube-apiserver  │◄─────────────►│       etcd       │   │
│   │ (REST API Gate)  │               │ (Key-Value DB)   │   │
│   └────────┬─────────┘               └──────────────────┘   │
│            │                                                │
│   ┌────────┴─────────┐               ┌──────────────────┐   │
│   │  kube-scheduler  │               │ controller-mgr   │   │
│   │ (Assigns Nodes)  │               │ (Self-Healing)   │   │
│   └──────────────────┘               └──────────────────┘   │
└──────────────────────────────┬──────────────────────────────┘
                               │ (Commands)
          ┌────────────────────┴────────────────────┐
          ▼                                         ▼
┌───────────────────────────┐             ┌───────────────────────────┐
│       WORKER NODE 1       │             │       WORKER NODE 2       │
│  ┌─────────────────────┐  │             │  ┌─────────────────────┐  │
│  │       kubelet       │  │             │  │       kubelet       │  │
│  │ (Node Captain)      │  │             │  │ (Node Captain)      │  │
│  └──────────┬──────────┘  │             │  └──────────┬──────────┘  │
│             ▼             │             │             ▼             │
│  ┌─────────────────────┐  │             │  ┌─────────────────────┐  │
│  │     kube-proxy      │  │             │  │     kube-proxy      │  │
│  │ (Network Director)  │  │             │  │ (Network Director)  │  │
│  └──────────┬──────────┘  │             │  └──────────┬──────────┘  │
│             ▼             │             │             ▼             │
│  ┌─────────────────────┐  │             │  ┌─────────────────────┐  │
│  │ [Pod] [Pod] [Pod]   │  │             │  │ [Pod] [Pod] [Pod]   │  │
│  │ containerd Runtime  │  │             │  │ containerd Runtime  │  │
│  └─────────────────────┘  │             │  └─────────────────────┘  │
└───────────────────────────┘             └───────────────────────────┘
```

### 🧠 Control Plane Components (The Brain)
| Component | Simple 1-Line Definition | Port | Real-Life Analogy |
| :--- | :--- | :---: | :--- |
| **`kube-apiserver`** | The front door of Kubernetes that receives all commands from `kubectl` or users, validates authentication, and coordinates the cluster. | `6443` | The front receptionist of a hospital taking all appointments. |
| **`etcd`** | A high-availability distributed key-value database storing the entire cluster state, configuration, and secrets. | `2379 / 2380` | The master safe vault holding all hospital patient records. |
| **`kube-scheduler`** | Watches for new pods and decides which physical worker node has enough free CPU and RAM to run them. | `10259` | The hotel room clerk assigning guests to clean, empty rooms. |
| **`kube-controller-manager`**| The background loop that constantly monitors the cluster and fixes deviations (e.g., if a pod dies, it orders a replacement). | `10257` | A security guard walking the halls making sure all doors stay locked. |
| **`cloud-controller-manager`**| Connects Kubernetes to cloud provider APIs (AWS, Azure, GCP) to automatically provision Cloud Load Balancers and EBS storage disks. | N/A | A liaison officer booking rental cars from external vendors. |

---

### 👷 Worker Node Components (The Muscle)
| Component | Simple 1-Line Definition | Port | Real-Life Analogy |
| :--- | :--- | :---: | :--- |
| **`kubelet`** | The agent running on every worker node that talks to the API server and ensures container runtimes pull images and run pods. | `10250` | The foreman on a construction floor executing instructions from headquarters. |
| **`kube-proxy`** | The network manager running on every node that updates iptables/IPVS rules to route service traffic to the right pod IP addresses. | `10256` | The traffic police officer directing cars at a busy intersection. |
| **Container Runtime (`containerd` / `CRI-O`)** | The low-level software engine that actually pulls container images and executes them on the Linux kernel. | Unix Socket | The physical engine inside a car that makes wheels turn. |

---

## 📦 3. Kubernetes Objects & Workloads (1-Line Definitions)

| Object Name | Simple 1-Line Definition (Kids Mind) | When to Use |
| :--- | :--- | :--- |
| **Pod** | The smallest deployable unit in Kubernetes, wrapping one or more containers that share the same IP, port space, and storage. | Always (every container runs inside a Pod). |
| **ReplicaSet** | Ensures an exact number of identical pod copies (replicas) are running at all times. | Managed automatically by Deployments. |
| **Deployment** | Manages stateless applications, providing automated rolling updates, rollbacks, and replica scaling with zero downtime. | Web servers, REST APIs, microservices. |
| **StatefulSet** | Manages stateful applications where pods need unique, persistent identities, fixed hostnames (`db-0`, `db-1`), and dedicated disks. | Databases (PostgreSQL, MySQL, MongoDB, Kafka, Redis). |
| **DaemonSet** | Ensures that exactly one copy of a Pod runs on **every single worker node** in the entire cluster. | Log collectors (`Fluent Bit`), metric agents (`Node Exporter`). |
| **Job** | Runs a batch container to completion (exit code 0) and then stops; does not restart it. | Database schema migrations, backup scripts, report generation. |
| **CronJob** | Runs a Job automatically on a repeating schedule (e.g., every night at midnight `0 0 * * *`). | Nightly database backups, weekly analytics cleanup. |

---

## 🌐 4. Networking & Service Types

Pods are ephemeral; their IP addresses change every time they restart. **Services** provide static, permanent IP addresses and DNS names that never change.

| Service Type | Simple 1-Line Definition | Traffic Scope | Port Number Range |
| :--- | :--- | :--- | :---: |
| **`ClusterIP`** | Default internal service type; gives pods a private virtual IP accessible **only inside** the cluster. | Internal Only | Custom (e.g., `80`, `5432`) |
| **`NodePort`** | Exposes the service on a dedicated static port on **every worker node's external IP address**. | External Access | **`30000 – 32767`** |
| **`LoadBalancer`**| Asks cloud provider (AWS/GCP/Azure) to spin up an external cloud load balancer pointing directly into the cluster. | Public Internet | Standard (`80`, `443`) |
| **`ExternalName`**| Acts as an internal CNAME DNS alias pointing to an external domain outside Kubernetes (e.g., `db.prod.aws.com`). | Outbound DNS | N/A |
| **`Ingress`** | An intelligent Layer 7 reverse proxy (like NGINX or Traefik) routing traffic based on hostnames (`app.com`) and URL paths (`/api`). | Public Internet | **`80, 443`** |
| **`NetworkPolicy`**| In-cluster firewall rules defining which pods are permitted to talk to other pods over TCP/UDP ports. | Security Guard | All Ports |

---

## 🗄️ 5. Storage & Configuration Objects

| Concept | Simple 1-Line Definition (Kids Mind) | Analogy |
| :--- | :--- | :--- |
| **`ConfigMap`** | Stores non-confidential configuration settings in plain text key-value pairs (e.g., `LOG_LEVEL: debug`). | A public whiteboard message. |
| **`Secret`** | Stores sensitive data (passwords, TLS certificates, API tokens) base64-encoded to prevent accidental leakage. | A locked office safe. |
| **`Volume`** | Ephemeral storage attached to a Pod that lives and dies with the Pod lifecycle. | A backpack you carry that gets thrown away when you leave. |
| **`PersistentVolume (PV)`** | A real piece of storage in the cluster (e.g., AWS EBS volume or NFS share) provisioned by administrators. | A physical storage locker in a warehouse. |
| **`PersistentVolumeClaim (PVC)`** | A voucher ticket submitted by a Pod requesting a specific amount of storage (e.g., "I need 20 GB"). | The rental claim ticket you hand to the locker manager. |
| **`StorageClass (SC)`** | The automated robot that dynamically creates real cloud disks (AWS gp3, Azure Managed Disk) the moment a PVC requests one! | An automated locker dispenser that builds new lockers on demand. |

---

## 📈 6. Autoscaling & Governance

| Concept | Simple 1-Line Definition | How It Works |
| :--- | :--- | :--- |
| **HPA (Horizontal Pod Autoscaler)** | Automatically increases or decreases the **number of Pod copies** based on CPU, memory, or custom traffic spikes. | Traffic rises ──► HPA scales pods from 3 to 30. |
| **VPA (Vertical Pod Autoscaler)** | Automatically adjusts the **CPU and RAM sizing limits** of existing pods to prevent memory crashes. | Gives a pod more RAM if it gets thirsty. |
| **Cluster Autoscaler / Karpenter** | Automatically adds or deletes **physical cloud VM worker nodes** to the cluster when pods cannot schedule. | Adds an extra EC2 instance in 45 seconds when nodes fill up. |
| **ResourceQuota** | Hard ceiling limits on the total CPU, RAM, or number of pods that can be created inside a specific Namespace. | Restricts Dev team to maximum 16 CPU cores and 32GB RAM. |
| **LimitRange** | Sets default minimum and maximum CPU/RAM boundaries for individual pods inside a Namespace. | Prevents a developer from creating an accidental 100-core pod. |
| **Namespace** | A virtual room dividing a single Kubernetes cluster into isolated workspaces (e.g., `dev`, `staging`, `prod`). | Different classrooms inside the same school building. |

---

## 🩺 7. Pod Health Probes & Lifecycle States

### 🔍 The 3 Health Probes
```yaml
livenessProbe:    # "Are you dead?" -> If fails, Kubernetes KILLS and restarts the pod!
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 15

readinessProbe:   # "Are you ready for customers?" -> If fails, K8s stops sending traffic to pod!
  httpGet:
    path: /ready
    port: 8080

startupProbe:     # "Did you finish booting up?" -> Protects slow-starting apps from premature kills.
  httpGet:
    path: /healthz
    port: 8080
  failureThreshold: 30
```

### 🚨 Common Pod Failure States (Troubleshooting Cheat Sheet)

| Status | Simple 1-Line Meaning | Immediate Fix Command |
| :--- | :--- | :--- |
| **`Pending`** | Pod cannot find a worker node with enough free CPU/RAM, or storage volume is stuck. | `kubectl describe pod <NAME>` (Check `Events:` section at bottom!) |
| **`CrashLoopBackOff`** | Container booted up, crashed immediately, and Kubernetes is backing off retries. | `kubectl logs <NAME> --previous` (Read the dying crash error message!) |
| **`ImagePullBackOff` / `ErrImagePull`** | Docker image name is misspelled, tag doesn't exist, or registry secret is missing. | Verify image name, tag, and docker-registry secret in YAML. |
| **`OOMKilled` (Exit Code 137)** | The container exceeded its memory limit (`resources.limits.memory`) and the Linux kernel killed it! | Increase `resources.limits.memory` in the pod deployment manifest. |
| **`NodeNotReady`** | The worker node's kubelet crashed, disk is 100% full, or network connection died. | SSH into node and run `systemctl status kubelet` and `df -h`. |

---

---

## 🔐 8. Kubernetes Security & RBAC (1-Line Definitions)

| Concept | Simple 1-Line Definition (Kids Mind) | Real-Life Analogy |
| :--- | :--- | :--- |
| **`ServiceAccount`** | An identity badge given to a Pod so its code can talk directly to the Kubernetes API. | An ID badge given to an automated cleaning robot. |
| **`Role`** | A permission rule list defining what actions are allowed inside a **single specific namespace**. | Room key granting access only to Classroom 3A. |
| **`ClusterRole`** | A cluster-wide permission rule list applying across **all namespaces** (e.g., ability to view all nodes). | Master master key opening every room in the school building. |
| **`RoleBinding`** | Grants the permissions in a `Role` to a specific user, group, or ServiceAccount in a namespace. | Handing the Classroom 3A key to student Alice. |
| **`ClusterRoleBinding`** | Grants cluster-wide `ClusterRole` permissions to a user or ServiceAccount everywhere. | Handing the Master key to the building superintendent. |

---

## 🧭 9. Advanced Pod Scheduling: Taints, Tolerations & Affinity

| Feature | Simple 1-Line Definition | Real-Life Analogy |
| :--- | :--- | :--- |
| **Taint** | A spray applied to a Node to **repel** pods from being scheduled on it unless they have permission. | A "Wet Paint" warning sign on a park bench. |
| **Toleration** | A special badge on a Pod allowing it to sit on a tainted Node despite the warning. | A worker wearing plastic overalls who can sit on wet paint. |
| **Node Affinity** | A rule on a Pod stating it **prefers or requires** nodes with specific hardware labels (e.g., `gpu=true`). | A passenger requesting a window seat with extra legroom. |

---

## ⎈ 10. Helm — The Kubernetes Package Manager

- **What is Helm? (1-Line Definition):** The package manager for Kubernetes (like `apt` for Linux or `npm` for Node.js) that packages complex apps into versioned templates.
- **Key Concepts:**
  - **Helm Chart:** A folder containing YAML templates and default variables needed to install an app.
  - **`values.yaml`:** The single configuration file where you customize settings (e.g., replica count, disk size, image tag).
  - **Release:** A running instance of a chart inside the cluster.
- **Essential Helm Commands:**
  - `helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx`: Adds a chart repository.
  - `helm repo update`: Updates local cached charts.
  - `helm install my-nginx ingress-nginx/ingress-nginx -f custom-values.yaml`: Installs the application.
  - `helm upgrade my-nginx ingress-nginx/ingress-nginx --set replicaCount=5`: Updates settings.
  - `helm rollback my-nginx 1`: Instantly rolls back to revision 1 if the upgrade fails!
  - `helm uninstall my-nginx`: Cleans up all resources created by the chart.

---

## 💻 11. Essential `kubectl` Commands & Screen Output Breakdown

### 1️⃣ `kubectl get pods -A -o wide` (Cluster Pod Inventory)
- **1-Line Definition:** Lists all pods across every namespace with their IP addresses, node locations, and restart counts.
- **What details appear on screen:**
```text
NAMESPACE     NAME                      READY   STATUS    RESTARTS   AGE   IP            NODE          NOMINATED NODE
kube-system   coredns-768b85b76-4d2p7   1/1     Running   0          14d   10.244.0.2    node-worker-1 <none>
prod          web-app-5d8f6b896f-8k9w2  1/1     Running   0          2h    10.244.1.45   node-worker-2 <none>
prod          api-backend-79d8c-z9q1l   0/1     CrashLoop 5 (2m ago) 15m   10.244.1.46   node-worker-2 <none>
```
- `READY 1/1`: 1 container requested, 1 container passing readiness probes.
- `STATUS`: Health state (Running, Pending, CrashLoopBackOff).
- `RESTARTS`: How many times the container crashed and was revived by kubelet.
- `IP`: Internal cluster pod IP address.
- `NODE`: The exact physical or VM worker node running this pod.

---

### 2️⃣ `kubectl describe pod <NAME> -n <NAMESPACE>` (The Ultimate Debugger)
- **1-Line Definition:** Dumps comprehensive metadata, volume mounts, conditions, and the chronological event timeline of a pod.
- **Where to look:** Scroll straight to the bottom `Events:` table:
```text
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  3m                 default-scheduler  Successfully assigned prod/web-app to node-worker-2
  Normal   Pulling    3m                 kubelet            Pulling image "nginx:1.25"
  Normal   Pulled     2m45s              kubelet            Successfully pulled image "nginx:1.25"
  Warning  Failed     30s (x4 over 2m)   kubelet            Error: ImagePullBackOff / OOMKilled
```

---

### 3️⃣ Everyday Cheat Sheet Commands
```bash
# Stream live logs from pod
kubectl logs -f my-pod -n prod

# Jump inside a running pod shell
kubectl exec -it my-pod -n prod -- /bin/sh

# Apply declarative YAML manifests
kubectl apply -f ./k8s-manifests/

# Check real-time CPU and Memory usage
kubectl top nodes
kubectl top pods -A

# Zero-downtime rolling restart
kubectl rollout restart deployment my-app -n prod

# Instant rollback of a bad release
kubectl rollout undo deployment my-app -n prod

# Cordon & Drain node for maintenance
kubectl cordon node-1
kubectl drain node-1 --ignore-daemonsets --delete-emptydir-data
kubectl uncordon node-1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Docker Short Notes](./04-Docker-and-Containers-Short-Notes.md) | [Index](../README.md) | [Cloud Computing (AWS/Azure/GCP) Short Notes →](./06-Cloud-Computing-AWS-Azure-GCP-Short-Notes.md) |
