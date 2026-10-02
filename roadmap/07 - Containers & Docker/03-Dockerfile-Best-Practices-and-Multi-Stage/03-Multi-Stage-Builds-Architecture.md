# 03 - Multi-Stage Builds Architecture

## 1. Decoupling Build Toolchains from Production Runtime

Compiling code (Go, Rust, TypeScript, Java) requires heavy SDKs, compilers, git, and header files (often 1.5GB+).
**Multi-stage builds** use temporary build stages to compile binaries, copying only the final compiled artifact into an ultra-lean runtime base.

```dockerfile
# Stage 1: Build & Compile
FROM golang:1.22-alpine AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
# Compile static CGO-disabled binary
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o /bin/app .

# Stage 2: Final Production Runtime (Zero SDKs, Zero Compilers!)
FROM gcr.io/distroless/static-debian12
WORKDIR /app
COPY --from=builder /bin/app /app/app
USER nonroot:nonroot
ENTRYPOINT ["/app/app"]
```

### Result:
- Image size drops from **1.2 GB** down to **18 MB**!
- Zero package managers (`apt`, `apk`), eliminating CVE vulnerabilities.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Layer Caching Optimization](./02-Layer-Caching-Optimization-and-Build-Order.md) | [README](./README.md) | [04 - Distroless, Alpine & Scratch](./04-Distroless-Alpine-and-Scratch-Base-Images.md) |
