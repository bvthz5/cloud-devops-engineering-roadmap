# 01 - Gateway API vs Ingress: The Evolution of L7 Routing

## 1. Why Was Ingress Replaced? (The Historical Context)

The original Kubernetes **`Ingress`** API was introduced in 2015 (Kubernetes 1.1) as a lightweight, "minimum viable" specification to expose HTTP and HTTPS services to external traffic outside the cluster. 

While Ingress succeeded in standardizing basic host-based (`api.example.com`) and path-based (`/users`) routing, it quickly proved inadequate as enterprise Kubernetes adoption accelerated. Ingress suffered from four fundamental architectural flaws:

1. **Monolithic Responsibility Model:** The `Ingress` resource conflated infrastructure concerns (external IP allocation, listener ports 80/443, TLS certificate bindings) with application routing rules (URI paths, microservice endpoints). In reality, these concerns belong to completely different organizational personas: cluster platform engineers manage load balancers and domain certificates, while application developers manage routing paths.
2. **The "Annotation Sprawl" Nightmare:** The core Ingress specification lacked native fields for advanced L7 routing features such as URL path rewrites, header modification, CORS policies, rate limiting, and canary traffic splitting. As a consequence, every ingress controller vendor (NGINX, Traefik, HAProxy, AWS ALB) invented hundreds of proprietary annotations:
   ```yaml
   nginx.ingress.kubernetes.io/rewrite-target: /$2
   alb.ingress.kubernetes.io/target-type: ip
   traefik.ingress.kubernetes.io/router.middlewares: auth-headers@kubernetescrd
   ```
   This annotation sprawl destroyed portability. Switching an ingress controller from NGINX to Traefik or Envoy required rewriting every single Ingress YAML manifest in the organization.
3. **No Cross-Namespace Routing:** An Ingress resource could only route traffic to backend Services located **in the exact same namespace**. If an enterprise ran 50 microservices across 50 separate namespaces, platform engineers were forced to provision 50 separate cloud load balancers (incurring massive cloud costs) or implement brittle external-name hacks.
4. **Zero Native Support for Non-HTTP Protocols:** Ingress was fundamentally hardcoded to HTTP/HTTPS. It offered no native mechanisms for gRPC streams, raw TCP sockets, or UDP traffic.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE INGRESS DILEMMA VS GATEWAY API PERSONA SEPARATION                      |
+---------------------------------------------------------------------------------------------------+

 LEGACY INGRESS: MONOLITHIC & BRITTLE
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Ingress Resource (Single YAML Manifest)                                          │
 │  ├── Infrastructure: TLS Secrets, External IP, Ports (Managed by Platform Team) │
 │  ├── Routing: Host rules, /api /v1 paths              (Managed by Dev Team)     │
 │  └── Vendor Annotations: 40+ non-portable string flags (nginx.ingress...)         │
 └──────────────────────────────────────────────────────────────────────────────────┘
 Bottleneck: Devs need cluster-wide permissions; switching controllers breaks all YAML!

 MODERN GATEWAY API: CLEAN ROLE-ORIENTED ARCHITECTURE
 ┌────────────────────────────────────────────────────────┐
 │ 1. GatewayClass (Infrastructure Provider Persona)      │  Managed by Cloud / CNI Vendor
 │    (Defines controller: Envoy Gateway, Cilium, Istio)  │
 └───────────────────────────┬────────────────────────────┘
                             │
                             ▼
 ┌────────────────────────────────────────────────────────┐
 │ 2. Gateway (Cluster Operator / Platform Persona)       │  Managed by Platform Team
 │    (Defines Listeners, Port 443, Wildcard TLS Secrets) │  in "infra-gateway" namespace
 └───────────────────────────┬────────────────────────────┘
                             │ Cross-Namespace Attachment
         ┌───────────────────┴───────────────────┐
         ▼                                       ▼
 ┌───────────────────────────────┐       ┌───────────────────────────────┐
 │ 3. HTTPRoute (Billing Devs)   │       │ 3. HTTPRoute (Search Devs)    │
 │    Namespace: "billing"       │       │    Namespace: "search"        │
 │    Matches: /billing/*        │       │    Matches: /search/*         │
 └───────────────────────────────┘       └───────────────────────────────┘
```

---

## 2. The 3 Role-Oriented Personas of Gateway API

The **Gateway API** (governed by Kubernetes SIG Network, achieving General Availability in Kubernetes 1.25+) replaces the monolithic Ingress model with a modular, role-oriented hierarchy:

| Persona | Role in Organization | Kubernetes API Object | Scope & Responsibility |
| :--- | :--- | :--- | :--- |
| **Infrastructure Provider** | Cloud Provider / Vendor (AWS, Google Cloud, Envoy, Cilium) | **`GatewayClass`** | Cluster-scoped. Defines the underlying controller implementation and capabilities. |
| **Cluster Operator** | Platform Engineer / DevOps / SRE | **`Gateway`** | Namespaced (usually in `infra` or `gateway-system`). Defines network listeners, public IP bindings, ports (80, 443, 8443), and corporate TLS certificates. |
| **Application Developer** | Software Engineer / Microservice Team | **`HTTPRoute`**, **`GRPCRoute`**, **`TCPRoute`** | Namespaced within application project. Attaches to the shared Gateway; declares request matching, header routing, and canary traffic weights. |

---

## 3. Production Manifests: Persona-Driven Gateway API Setup

### Step 1: The Platform Engineer Creates the Shared Gateway
The platform engineer exposes port 443 with a corporate wildcard TLS certificate and permits routes from application namespaces to attach:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: enterprise-gateway
  namespace: infra-gateway
spec:
  gatewayClassName: envoy-gateway
  listeners:
    - name: https
      protocol: HTTPS
      port: 443
      hostname: "*.enterprise.com"
      tls:
        mode: Terminate
        certificateRefs:
          - kind: Secret
            name: wildcard-enterprise-tls
      allowedRoutes:
        namespaces:
          from: Selector
          selector:
            matchLabels:
              gateway-access: "enabled"
```

### Step 2: The Application Team Deploys an HTTPRoute with Native Canary Splitting
In the `payments` namespace, the developer creates an `HTTPRoute` that attaches to `infra-gateway/enterprise-gateway`. Notice that canary traffic splitting is **100% native** without requiring any proprietary annotations:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: payment-routing
  namespace: payments
spec:
  parentRefs:
    - name: enterprise-gateway
      namespace: infra-gateway
  hostnames:
    - "payments.enterprise.com"
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /api/v2
      backendRefs:
        # Route 90% of traffic to stable v1 backend
        - name: payment-service-v1
          port: 8080
          weight: 90
        # Route 10% of traffic to canary v2 backend
        - name: payment-service-v2
          port: 8080
          weight: 10
```

---

## 4. Cross-Namespace Security: The `ReferenceGrant` Handshake

Under the legacy Ingress model, an application in one namespace could accidentally or maliciously hijack routes belonging to another namespace.

Gateway API introduces a cryptographic cross-namespace security handshake called **`ReferenceGrant`**. If a Gateway in `infra-gateway` needs to reference a TLS Secret stored in `security-vault`, or an HTTPRoute in `payments` wants to forward traffic to a Service in `shared-cache`, the destination namespace must explicitly grant permission:

```yaml
apiVersion: gateway.networking.k8s.io/v1beta1
kind: ReferenceGrant
metadata:
  name: allow-gateway-tls-access
  namespace: security-vault
spec:
  from:
    - group: gateway.networking.k8s.io
      kind: Gateway
      namespace: infra-gateway
  to:
    - group: ""
      kind: Secret
      name: wildcard-enterprise-tls
```
Without this explicit `ReferenceGrant`, the Gateway API controller strictly forbids cross-namespace object references.

---

## 5. Architectural Feature Matrix: Ingress vs Gateway API

| Feature / Capability | Legacy Ingress (`networking.k8s.io/v1`) | Modern Gateway API (`gateway.networking.k8s.io/v1`) |
| :--- | :--- | :--- |
| **Separation of Concerns** | Monolithic (single YAML file) | **3-Tier Role-Oriented** (Class, Gateway, Route) |
| **Cross-Namespace Routing** | **Unsupported** | **Fully Supported** with `ReferenceGrant` security |
| **Canary Traffic Splitting** | Requires proprietary vendor annotations | **Native first-class feature** (`weight: 90 / 10`) |
| **Header Matching & Rewrites** | Vendor annotations only | **Native, standardized declarative fields** |
| **Supported Protocols** | HTTP / HTTPS only | **HTTP, HTTPS, gRPC, TCP, UDP, and TLS** |
| **Multi-Tenancy** | Weak; developers can overwrite hostnames | **Strong RBAC isolation** per persona |
| **Specification Portability**| Zero; heavy annotation lock-in | **100% portable across Envoy, Cilium, Istio, Cloud** |

---

## 6. Operational Inspection Commands

```bash
# Check available GatewayClasses in the cluster
kubectl get gatewayclasses

# Inspect active Gateways and their assigned external Load Balancer IP addresses
kubectl get gateways -n infra-gateway

# List all HTTPRoutes and their parent Gateway attachment status
kubectl get httproutes -A

# Inspect detailed route resolution, listener status, and attachment conditions
kubectl describe gateway enterprise-gateway -n infra-gateway
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (17-Advanced-Scheduling-Taints-Tolerations-and-Affinity)](../17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Gateway API Role Oriented Architecture GatewayClass Gateway Route →](./02-Gateway-API-Role-Oriented-Architecture-GatewayClass-Gateway-Route.md) |
