# Interview Q&A - Google Cloud Run & Cloud Functions

> **Module**: Google Cloud Run & Cloud Functions

---

### Q1: What is the difference between GKE Standard and GKE Autopilot?
**Answer**:
- **GKE Standard**: Provides full control over node management, node pools, OS configuration, and cluster sizing. You manage and pay for node infrastructure.
- **GKE Autopilot**: Fully managed hands-off Kubernetes experience where Google manages cluster node infrastructure, security hardening, and scaling. You pay only for Pod resource requests (CPU, memory, storage) rather than VM nodes.

---

### Q2: How does Cloud Run compare to Cloud Functions?
**Answer**:
- **Cloud Run**: Runs stateless HTTP containers listening on any port. Supports any programming language, binary dependency, or custom base image.
- **Cloud Functions**: Event-driven serverless FaaS (Function-as-a-Service) executing specific code snippets triggered by events (Pub/Sub, GCS events). Gen2 Cloud Functions are built on top of Cloud Run and Eventarc behind the scenes.

---

### Q3: What is Binary Authorization in GCP?
**Answer**:
Binary Authorization is a deploy-time security control for GKE and Cloud Run. It ensures that only trusted container images signed by authorized attestors (e.g., passed CI/CD security vulnerability scans) can be deployed to production environments.
