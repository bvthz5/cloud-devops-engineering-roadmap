# Hands-On Practice - Google Cloud Operations (Cloud Monitoring & Logging)

> **Module**: Google Cloud Operations (Cloud Monitoring & Logging)

---

## 🛠 Lab: Deploying a Serverless Microservice with Cloud Run & Cloud Build

### Objective
In this lab, you will write a containerized microservice, build the Docker image using Cloud Build, store it in Artifact Registry, and deploy it to Cloud Run with HTTPS endpoint exposure.

---

## 📋 Task 1: Create Artifact Registry Repository
```bash
# Create Docker repository in Artifact Registry
gcloud artifacts repositories create app-repo \
    --repository-format=docker \
    --location=us-central1 \
    --description="Production Application Docker Repository"
```

---

## 📋 Task 2: Build & Push Container with Cloud Build
```bash
# Submit build to Cloud Build
gcloud builds submit --tag us-central1-docker.pkg.dev/$DEVSHELL_PROJECT_ID/app-repo/hello-service:v1 .
```

---

## 📋 Task 3: Deploy to Cloud Run & Verify
```bash
# Deploy container to Cloud Run
gcloud run deploy hello-service \
    --image=us-central1-docker.pkg.dev/$DEVSHELL_PROJECT_ID/app-repo/hello-service:v1 \
    --region=us-central1 \
    --platform=managed \
    --allow-unauthenticated

# Describe service endpoint
gcloud run services describe hello-service --region=us-central1 --format="value(status.url)"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
