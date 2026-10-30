# 07 — Real-World Production Scenarios: Compilers & Runtimes

---

## Scenario 1: The Alpine Scratch Container Failure

### Incident Summary
A developer builds a Go microservice and packages it into a minimal Docker container:
```dockerfile
FROM scratch
COPY app /app
ENTRYPOINT ["/app"]
```
When deployed to Kubernetes, the pod enters `CrashLoopBackOff` with:
`failed to create containerd task: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "/app": no such file or directory`.

### Root Cause Analysis
By default on Linux, Go uses `cgo` when compiling packages that import `net` (for local C resolver) or `os/user`.
The resulting binary was dynamically linked against `libc.so.6`.
Because `scratch` is a completely empty container image with no operating system or dynamic linker, the kernel cannot load the binary!

### Production Solution
Recompile with `CGO_ENABLED=0` to create a 100% statically linked ELF binary:
```bash
CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o app .
```
Verify with `file app`:
`app: ELF 64-bit LSB executable, x86-64, statically linked, stripped`.
The static binary runs perfectly in `scratch` with zero image dependencies!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Dynamic Linker and Symbol Resolution](./06-Dynamic-Linker-and-Symbol-Resolution.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
