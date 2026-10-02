# 10 - Hands-On Practice: Testing a gRPC Endpoint with grpcurl

## Lab Scenario
Explore and invoke a live gRPC service using the `grpcurl` command-line utility.

---

## Lab Steps

### Step 1: Install grpcurl
```bash
# On Linux/macOS via Go or Homebrew:
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

### Step 2: Query Public gRPC Test Service
```bash
# Inspect available methods on public test server
grpcurl grpcb.in:9000 list
grpcurl grpcb.in:9000 describe grpcbin.GRPCBin
```

### Step 3: Call Unary Echo Method
```bash
grpcurl -d '{"greeting": "Hello from DevOps!"}' grpcb.in:9000 grpcbin.GRPCBin/SayHello
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
