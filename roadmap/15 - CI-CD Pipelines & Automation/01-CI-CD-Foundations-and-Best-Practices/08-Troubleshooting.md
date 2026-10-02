# Troubleshooting Guide - CI/CD Foundations & Best Practices

> **Module**: CI/CD Foundations & Best Practices

---

## 🔍 Common Issue 1: Flaky CI Build Failures due to Uncached Dependencies

### Symptom
Pipeline jobs periodically fail with `NPM ERR! ETIMEDOUT` or `Maven dependency download timeout` when pulling dependencies from public registries.

### Root Cause & Resolution
1. **Registry Rate Limits / Network Outages**: Direct calls to public registries (`npmjs.org`, `repo.maven.apache.org`) hit rate limits.
2. **Private Artifact Proxy**: Route dependency downloads through an internal Nexus or JFrog Artifactory proxy repository.
3. **Enable Action Caching**: Configure step caching (`actions/cache` or `cache: 'npm'`) to store local packages on the runner host.

---

## 🔍 Common Issue 2: Docker-in-Docker (dind) Permission & Socket Failures in GitLab CI

### Symptom
GitLab CI job fails with `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`.

### Root Cause & Resolution
1. **Privileged Mode Disabled**: The GitLab Runner configuration `config.toml` lacks `privileged = true`.
2. **Alternative Container Builders**: Replace Docker-in-Docker with rootless, unprivileged container build tools like **Kaniko** or **Buildah**:
   ```yaml
   script:
     - /kaniko/executor --context $CI_PROJECT_DIR --dockerfile $CI_PROJECT_DIR/Dockerfile --destination $IMAGE_TAG
   ```
