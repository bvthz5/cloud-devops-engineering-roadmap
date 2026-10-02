# 04 - Systemd Integration and Podman Quadlets

## 1. Declarative Containers via Systemd Quadlets

Modern Podman integrates deeply with systemd using **Quadlets** (`.container` files). Place declarative files in `/etc/containers/systemd/` or `~/.config/containers/systemd/`:

```ini
# ~/.config/containers/systemd/web.container
[Unit]
Description=Production Web Gateway
After=network-online.target

[Container]
Image=nginx:alpine
PublishPort=8080:80
Volume=web_data:/var/www/html:ro

[Service]
Restart=always

[Install]
WantedBy=default.target
```

```bash
# Reload systemd and manage container like any native OS service!
systemctl --user daemon-reload
systemctl --user start web
systemctl --user status web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Podman Pods & Kube YAML](./03-Podman-Pods-and-Kubernetes-YAML-Generation.md) | [README](./README.md) | [05 - Buildah: Scriptable Builds](./05-Buildah-Scriptable-Daemonless-Image-Building.md) |
