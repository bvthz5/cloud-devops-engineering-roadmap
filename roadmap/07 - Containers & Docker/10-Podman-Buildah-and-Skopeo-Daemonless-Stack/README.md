# 10 - Podman, Buildah, and Skopeo Daemonless Stack

While Docker relies on a centralized background daemon (`dockerd`) running as root, the Red Hat/CNCF **daemonless container stack** decomposes container operations into three single-purpose, rootless utilities: **Podman** (container and pod execution), **Buildah** (scriptable image construction), and **Skopeo** (remote registry inspection and transfer without pulling).

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [The Daemonless Container Architecture](./01-The-Daemonless-Container-Architecture.md) | Why eliminate background daemons? Fork/exec process model, eliminating single points of failure. |
| 02 | [Rootless Podman & User Namespaces](./02-Rootless-Podman-and-User-Namespaces.md) | Rootless by default, `/etc/subuid` and `/etc/subgid`, user namespaces, slirp4netns vs pasta. |
| 03 | [Podman Pods & Kubernetes YAML Generation](./03-Podman-Pods-and-Kubernetes-YAML-Generation.md) | Pod abstractions, infra pause container, `podman generate kube`, `podman play kube`. |
| 04 | [Systemd Integration & Podman Quadlets](./04-Systemd-Integration-and-Podman-Quadlets.md) | Managing containers via systemd unit files, modern Quadlets (`.container` files), auto-start. |
| 05 | [Buildah: Scriptable Daemonless Builds](./05-Buildah-Scriptable-Daemonless-Image-Building.md) | Building images without a Dockerfile, bash automation (`buildah from`, `run`, `commit`). |
| 06 | [Skopeo: Remote Registry Operations](./06-Skopeo-Remote-Registry-Operations-Without-Pulling.md) | Copying images directly between remote registries without pulling to disk, signing, inspection. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Docker daemon crash cascading outages, Quadlet auto-restart failure. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging rootless subuid/subgid mapping errors, `podman unshare`, network latency in slirp4netns. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps questions on Podman vs Docker, daemonless architecture, and Quadlets. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Deploying a Podman pod; Lab 2: Production Quadlet systemd service; Lab 3: Skopeo copy. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for Podman CLI, Quadlet syntax, Buildah scripts, and Skopeo commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - OCI Standards](../09-OCI-Standards-and-Container-Runtimes/README.md) | [README](./README.md) | [01 - Daemonless Architecture](./01-The-Daemonless-Container-Architecture.md) |
