# 01 - Ingress Resource Specification and Path Routing

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the Kubernetes Ingress primitive, Layer-7 HTTP routing versus Layer-4 Services, Ingress Resource vs. Ingress Controller mechanics, Host-based and Path-based routing rules, and TLS termination.

---

## 🏛️ 1. What Is Ingress & Why Was It Created?

In standard Kubernetes networking, exposing an application to external internet traffic using a Service of `type: LoadBalancer` operates strictly at **Layer 4 (Transport Layer: TCP/UDP)**.

Operating at Layer 4 introduces two severe architectural and economic bottlenecks:
1. **The Cloud Cost Crisis:** On cloud providers (AWS, Azure, GCP), every single Service configured with `type: LoadBalancer` provisions a separate, dedicated cloud load balancer (such as an AWS Network Load Balancer). If an organization runs 50 microservices, maintaining 50 independent cloud load balancers costs thousands of dollars per month in redundant idle fees.
2. **Lack of HTTP Awareness:** A Layer-4 load balancer inspects only IP addresses and TCP port numbers. It cannot inspect HTTP request host headers, URL paths (`/api/v1` vs. `/api/v2`), HTTP request methods, cookies, or SSL certificates.

An **Ingress** solves both challenges by operating at **Layer 7 (Application Layer: HTTP/HTTPS)**. It acts as an intelligent, unified application gateway that multiplexes external client traffic across dozens or hundreds of internal `ClusterIP` microservices through a **single, shared public IP address**.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   LAYER-7 INGRESS ROUTING ARCHITECTURE                 │
└────────────────────────────────────────────────────────────────────────┘

 External Client Traffic (Single Public Cloud IP: 203.0.113.80:443)
                            │
                            ▼
 ┌──────────────────────────────────────────────────────────────────────┐
 │                      INGRESS CONTROLLER (Layer 7)                    │
 │  (Ingress-Nginx / Traefik / Envoy Reverse Proxy)                     │
 │  - Terminates TLS/SSL Encryption                                     │
 │  - Inspects HTTP Host Header & URL Path                              │
 └──────────────────────────┬───────────────────────────────────────────┘
                            │
         ┌──────────────────┴──────────────────┐
         ▼ (Host: api.company.com)             ▼ (Host: store.company.com)
 ┌───────────────────────────────┐     ┌────────────────────────────────┐
 │ Path-Based Routing Table:     │     │ Path-Based Routing Table:      │
 │                               │     │                                │
 │ ├── Path: /users              │     │ └── Path: / (Catch-All)        │
 │ │   └──► Service: users-svc   │     │     └──► Service: storefront   │
 │ │        (ClusterIP)          │     │          (ClusterIP)           │
 │ └── Path: /orders             │     └────────────────────────────────┘
 │     └──► Service: orders-svc  │
 │          (ClusterIP)          │
 └───────────────────────────────┘
```

---

## ⚙️ 2. The Ingress Resource vs. The Ingress Controller

A critical architectural distinction that every DevOps engineer must master is that **an Ingress manifest by itself does absolutely nothing**:

### 1. The Ingress Resource (`kind: Ingress`):
- The Ingress resource is merely a declarative data record stored in `etcd`.
- It defines a set of routing rules (e.g., "Requests for `api.company.com/users` should route to the `users-svc` Service on port 80").
- If you deploy an Ingress manifest into a cluster that does not have an active Ingress Controller running, the Ingress sits completely idle and no traffic is routed.

### 2. The Ingress Controller:
- The Ingress Controller is the **actual running reverse proxy application** (such as Ingress-Nginx, Traefik, HAProxy, Envoy Gateway, or the AWS Load Balancer Controller) deployed inside the cluster.
- The controller runs an infinite control loop watching the Kubernetes API server for Ingress objects.
- When an Ingress object is created or updated, the controller reads the rules, dynamically updates its internal reverse proxy configuration file (e.g. `nginx.conf`), and reloads its routing table without dropping active connections.

---

## 🔬 3. Ingress Specification Rules: Host vs. Path Routing

The `spec:` block of an Ingress manifest defines how incoming HTTP requests are inspected and dispatched:

### 3.1 Host-Based Virtual Hosting:
Rules can be scoped to specific domain names via the `host:` field:
- Requests with `Host: api.company.com` route to API microservices.
- Requests with `Host: dashboard.company.com` route to administrative web applications.
- Requests matching no declared host can be captured by a default backend.

### 3.2 Path-Based Routing and Path Types (`pathType`):
Within a host, requests are routed based on URL paths. Kubernetes v1.18+ mandates an explicit **`pathType`** to eliminate ambiguities:

| Path Type | Matching Behavior & Real-World Mechanics |
| :--- | :--- |
| **`Prefix`** | Matches URL paths split by `/` character boundaries. For example, a prefix of `/v1` will match `/v1`, `/v1/`, and `/v1/users`, but will **not** match `/v10` or `/v1alpha`. |
| **`Exact`** | Matches the URL path strictly, identically, and case-sensitively. A path of `/login` will match only `/login`, and will reject `/login/` or `/login/callback`. |
| **`ImplementationSpecific`** | The matching algorithm is delegated entirely to the specific Ingress Controller implementation (e.g. regex evaluation in Ingress-Nginx). |

---

## 🔒 4. SSL/TLS Termination

Ingress controllers natively manage **SSL/TLS Termination**, decrypting incoming HTTPS traffic at the cluster perimeter and forwarding unencrypted HTTP traffic to internal microservices:

```yaml
spec:
  tls:
  - hosts:
    - api.company.com
    secretName: api-company-tls-secret
```

### How TLS Secrets Work:
The referenced Secret (`api-company-tls-secret`) must reside in the **exact same namespace as the Ingress resource**. It must be of type `kubernetes.io/tls` and contain two base64-encoded keys:
1. `tls.crt`: The public SSL certificate (including intermediate CA certificate bundles).
2. `tls.key`: The private encryption key.

When paired with **`cert-manager`**, Kubernetes automatically issues and renews free, trusted Let's Encrypt certificates directly into these Secrets!

---

## 📝 5. Production Ingress Manifest Template

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: enterprise-api-gateway
  namespace: production
  annotations:
    # Tell Ingress-Nginx to enforce HTTPS redirection
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    # Set client upload size limit
    nginx.ingress.kubernetes.io/proxy-body-size: "50m"
    # Rate limit: Max 100 requests per minute per client IP
    nginx.ingress.kubernetes.io/limit-rps: "100"
spec:
  # Mandatory: Targets the designated IngressClass controller
  ingressClassName: nginx
  
  # TLS Termination block
  tls:
  - hosts:
    - api.company.com
    secretName: enterprise-api-tls
    
  # Application routing rules
  rules:
  - host: api.company.com
    http:
      paths:
      # Route 1: Users Microservice
      - path: /api/v1/users
        pathType: Prefix
        backend:
          service:
            name: users-service
            port:
              number: 80
      # Route 2: Orders Microservice
      - path: /api/v1/orders
        pathType: Prefix
        backend:
          service:
            name: orders-service
            port:
              number: 80
```

---

## 📋 6. Summary and Navigation

The Ingress specification provides a scalable, cost-effective Layer-7 routing layer for Kubernetes. By decoupling declarative routing rules from the underlying reverse proxy controller, supporting host and path pattern matching, and automating SSL termination, Ingress delivers modern web gateway capabilities across distributed enterprise architectures.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Services-and-Service-Discovery)](../05-Services-and-Service-Discovery/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Ingress Nginx Architecture and Controller Mechanics →](./02-Ingress-Nginx-Architecture-and-Controller-Mechanics.md) |
