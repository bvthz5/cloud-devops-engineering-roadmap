# Troubleshooting Guide - Automated Testing Pipelines

> **Module**: Automated Testing Pipelines

---

## 🔍 Common Issue 1: Database Migration Lock & Rollout Deadlock

### Symptom
A rolling deployment stalls on new pods while database migration scripts (Liquibase / Flyway) fail with `Table lock timeout` or `Column does not exist`.

### Root Cause & Resolution
1. **Breaking Schema Change**: The new application version dropped a database column while the old running pods were still reading/writing to that column.
2. **Expand-Contract Pattern**: Split migrations into 3 distinct phases:
   - **Phase 1 (Expand)**: Add new column/table without removing old structures.
   - **Phase 2 (Deploy)**: Deploy application code writing to both old and new schema.
   - **Phase 3 (Contract)**: Drop old column in a subsequent release after verification.

---

## 🔍 Common Issue 2: Docker Build Cache Invalidation in CI Runners

### Symptom
Container image builds take 15 minutes on every run because Docker rebuilds all layers from scratch, ignoring previous layer caches.

### Root Cause & Resolution
1. **Ephemerality**: Ephemeral runner hosts do not preserve local Docker layer caches between job runs.
2. **Inline / Remote Cache**: Enable BuildKit and remote cache export/import options:
   ```bash
   docker buildx build      --cache-from=type=registry,ref=myregistry/app:buildcache      --cache-to=type=registry,ref=myregistry/app:buildcache,mode=max      --push -t myregistry/app:latest .
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
