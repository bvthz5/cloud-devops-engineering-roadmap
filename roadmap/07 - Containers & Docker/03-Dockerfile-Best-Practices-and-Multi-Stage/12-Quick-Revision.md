# 12 - Quick-Revision & Enterprise Cheat Sheet

```dockerfile
# Multi-Stage Golden Standard
FROM golang:1.22-alpine AS build
WORKDIR /app
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /bin/app

FROM gcr.io/distroless/static-debian12
COPY --from=build /bin/app /app
USER nonroot:nonroot
ENTRYPOINT ["/app"]
```

```bash
# Enable BuildKit
export DOCKER_BUILDKIT=1

# Analyze image layer efficiency
dive <image-name>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [04 - Docker Storage & Volumes](../04-Docker-Storage-and-Volumes/README.md) |
