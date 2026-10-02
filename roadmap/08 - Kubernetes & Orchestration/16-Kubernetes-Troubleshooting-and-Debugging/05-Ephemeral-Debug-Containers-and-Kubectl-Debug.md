# 05 - Ephemeral Debug Containers and Kubectl Debug

## 1. Debugging Distroless & Scratch Containers

Modern production containers run on **Distroless** or **Scratch** images with zero shell, zero `curl`, and zero debugging utilities.
`kubectl debug` attaches an **Ephemeral Container** containing full Linux utilities directly into the running Pod's namespaces without restarting it!

```bash
# 1. Attach ephemeral debug container sharing network and IPC with troubled pod
kubectl debug -it target-pod --image=nicolaka/netshoot --target=app-container

# 2. Debug a worker node by spawning a privileged pod on the host filesystem!
kubectl debug node/worker-01 -it --image=busybox:musl
# Inside container:
chroot /host
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Control Plane Diagnostics](./04-Control-Plane-Diagnostics-API-Server-and-etcd-Failures.md) | [README](./README.md) | [06 - Audit & Forensics](./06-Cluster-Auditing-Event-Analysis-and-Forensics.md) |
