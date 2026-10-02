# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the operational difference between `ARG` and `ENV` in a Dockerfile?
**Answer**: `ARG` defines variables available exclusively during image build time; they are not accessible when the container runs and should never be used for secrets because they persist in image layer history. `ENV` defines environment variables that persist both during the build and inside the running container environment.

### Q2: Why should `ENTRYPOINT` and `CMD` be written in Exec form rather than Shell form?
**Answer**: Exec form (`["executable", "param"]`) executes the binary directly as PID 1, allowing it to receive operating system signals (`SIGTERM`) for graceful shutdown. Shell form (`CMD myapp`) wraps the command in `/bin/sh -c`, which absorbs signals and causes timeouts and forced `SIGKILL` termination.

### Q3: How do multi-stage builds enhance container security?
**Answer**: Multi-stage builds leave compilers, package managers, development headers, and git binaries behind in intermediate builder stages, producing a lean production image containing only the compiled binary and essential runtime libraries. This drastically shrinks the attack surface and eliminates CVEs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
