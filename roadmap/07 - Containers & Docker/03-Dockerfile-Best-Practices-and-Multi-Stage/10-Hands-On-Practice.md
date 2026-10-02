# 10 - Hands-On Practice Labs

## Lab 1: Multi-Stage Production Go Binary on Distroless

### Objective
Create a multi-stage Dockerfile that compiles a Go web server and runs it on Google Distroless as a non-root user.

### Implementation (`Dockerfile`)
```dockerfile
# Stage 1: Build Environment
FROM golang:1.22-alpine AS builder
WORKDIR /src
COPY go.mod ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o /bin/server .

# Stage 2: Distroless Runtime
FROM gcr.io/distroless/static-debian12
WORKDIR /app
COPY --from=builder /bin/server /app/server
USER 65532:65532
EXPOSE 8080
ENTRYPOINT ["/app/server"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
