# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Podman Aliases & Pods
alias docker=podman
podman pod create --name mypod -p 80:80
podman generate kube mypod > pod.yaml

# Skopeo Direct Copy
skopeo copy docker://regA/app:v1 docker://regB/app:v1

# Buildah Scriptable Build
c=$(buildah from alpine)
buildah run $c apk add python3
buildah commit $c myimage
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [11 - Container Observability & Troubleshooting](../11-Container-Observability-and-Troubleshooting/README.md) |
