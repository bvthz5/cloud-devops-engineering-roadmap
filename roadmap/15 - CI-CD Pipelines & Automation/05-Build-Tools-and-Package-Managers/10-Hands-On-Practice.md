# Hands-On Practice - Build Tools & Package Managers

> **Module**: Build Tools & Package Managers

---

## 🛠 Lab: Multi-Stage Docker Build Optimization & BuildKit Caching

### Objective
In this lab, you will write a multi-stage Dockerfile for a Node.js/Go application, optimize layer caching, and verify container image reduction using `docker buildx`.

---

## 📋 Task 1: Create Multi-Stage Dockerfile
```dockerfile
# Stage 1: Build & Compile
FROM golang:1.20-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Stage 2: Minimal Runtime Image
FROM alpine:3.18 AS final
WORKDIR /app
RUN adduser -D appuser && chown -R appuser /app
COPY --from=builder /app/main .
USER appuser
EXPOSE 8080
CMD ["./main"]
```

---

## 📋 Task 2: Build with BuildKit Caching
```bash
# Enable Docker BuildKit
export DOCKER_BUILDKIT=1

# Execute build with inline cache
docker build --build-arg BUILDKIT_INLINE_CACHE=1 -t myapp:optimized .
```

---

## 📋 Task 3: Inspect Image Size Reduction
```bash
# Compare container image sizes
docker images myapp:optimized
```
