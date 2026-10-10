# 02 - Agentless Architecture & the SSH Push Model

In the history of configuration management, the debate between **Push** and **Pull** architectures defines how systems scale, maintain security boundaries, and respond to operational emergencies. Ansible's core innovation was proving that an **agentless, push-based model** operating over standard OpenSSH transports provides superior security, immediate execution, and zero idle overhead compared to daemon-heavy pull architectures like Puppet and Chef.

---

## 1. Push vs. Pull: Architectural Paradigms

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          PUSH VS. PULL ARCHITECTURES                            │
│                                                                                 │
│   ANSIBLE PUSH MODEL (On-Demand / Real-Time)                                    │
│   ┌───────────────────────┐                                                     │
│   │ Control Node / CI     │────── SSH Push (Instant Execution) ───────┐         │
│   │ (Initiator of Action) │                                           │         │
│   └───────────────────────┘                                           ▼         │
│                                                             ┌───────────────────┐
│                                                             │ Managed Node      │
│                                                             │ (Zero Daemon)     │
│                                                             └───────────────────┘
│                                                                                 │
│   PUPPET / CHEF PULL MODEL (Autonomous Polling)                                 │
│   ┌───────────────────────┐                                                     │
│   │ Central Master Server │◄───── Polling Check-in (Every 30 Mins) ───┐         │
│   │ (Passively Awaits)    │                                           │         │
│   └───────────────────────┘                                           │         │
│                                                             ┌─────────┴─────────┐
│                                                             │ Managed Node      │
│                                                             │ (Permanent Daemon)│
│                                                             └───────────────────┘
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The SRE Crisis That Favored Push: Emergency Zero-Day Patching
Imagine a critical vulnerability (e.g., OpenSSL Heartbleed, Log4Shell, or a sudo privilege escalation exploit) announced at 2 PM on a Friday:
- **With a Pull System (Puppet/Chef):** Target nodes check in periodically on random 30-minute intervals. Forcing an immediate patch across 2,000 servers requires waiting for all nodes to cycle through their timers, or running complex out-of-band SSH scripts to force the client daemon to run.
- **With Ansible's Push Model:** An engineer executes a single ad-hoc command:
  ```bash
  ansible all -m package -a "name=openssl state=latest" --forks=100
  ```
  The control node pushes the update across 2,000 servers concurrently in under 90 seconds, reporting back real-time stdout confirmation per host.

---

## 2. In-Depth Comparison Matrix: Agentless Push vs. Agent-Based Pull

| Architectural Dimension | Ansible (Agentless Push) | Puppet / Chef / Salt (Agent Pull) |
| :--- | :--- | :--- |
| **Node Resource Footprint** | **0 MB RAM / 0% Idle CPU** (Only standard `sshd` runs) | 50MB–200MB RAM permanently consumed per VM |
| **Network Attack Surface** | **Zero additional open ports** (Reuses existing SSH port 22) | Requires opening custom inbound ports (Puppet: 8140, Salt: 4505/4506) |
| **Certificate Lifecycle** | Reuses standard SSH keys or OpenSSH Certificate Authorities | Requires complex x509 PKI management and per-node SSL cert signing |
| **Execution Timing** | **Deterministic & Real-Time** (Executes when triggered) | Non-deterministic (Batched by polling timer) |
| **Blast Radius of Master Outage** | Low (Nodes continue operating normally; only new deployments pause) | High (Agents can fail or become unstable if Master certs expire) |
| **Bootstrapping Effort** | **Instant** (Any node accessible via SSH is immediately manageable) | High (Requires agent installation and initial cert handshakes) |

---

## 3. How the OpenSSH Push Transport Functions Mechanically

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       SSH PUSH CONNECTION SEQUENCE                              │
│                                                                                 │
│   CONTROL NODE                                            MANAGED NODE          │
│        │                                                        │               │
│    1.  │──── OpenSSH TCP Handshake (Port 22) ──────────────────►│               │
│        │◄─── Host Key Verification (ED25519) ───────────────────┤               │
│    2.  │──── Public Key Authentication (ansible_ed25519) ──────►│               │
│        │◄─── Session Authenticated ─────────────────────────────┤               │
│    3.  │──── Open SFTP Subsystem: Upload Zip Payload ──────────►│               │
│        │     (Uploaded to ~/.ansible/tmp/ansible-tmp-xxxx/)     │               │
│    4.  │──── Invoke: python3 ~/.ansible/tmp/xxxx/module.py ────►│               │
│        │     (Script executes module logic & captures state)    │               │
│    5.  │◄─── JSON Payload Returned on stdout ───────────────────┤               │
│    6.  │──── SSH Command: rm -rf ~/.ansible/tmp/xxxx/ ─────────►│               │
│    7.  │──── Close SSH Channel ────────────────────────────────►│               │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### SSH Connection Multiplexing: `ControlMaster` & `ControlPersist`
By default, opening an SSH connection involves TCP handshakes, TLS/SSH key exchanges, and authentication handshakes taking between **200ms and 500ms per task**. If a playbook runs 20 tasks on 50 servers, opening 1,000 separate SSH connections introduces massive network latency!

Ansible solves this using OpenSSH **ControlMaster socket multiplexing**:

```ini
# Production ssh_connection settings in ansible.cfg
[ssh_connection]
# ControlMaster keeps a shared Unix domain socket open for background reuse
ssh_args = -C -o ControlMaster=auto -o ControlPersist=15m -o PreferredAuthentications=publickey

# Pipelining streams Python scripts directly into stdin, bypassing SFTP disk writes!
pipelining = True
```
- **`ControlMaster=auto`:** The first task to a server creates a master Unix domain socket in `/tmp/ansible-ssh-<host>`.
- **`ControlPersist=15m`:** Keeps the authenticated socket alive for 15 minutes after the task finishes. Subsequent tasks connect in **< 10ms**, achieving near-instantaneous execution speed!

---

## 4. Scaling the Push Model: `ansible-pull` for Massive Fleets

While the push model is optimal for clusters up to several thousand servers, managing **10,000+ to 100,000+ edge nodes** or dynamic auto-scaling fleets can strain the Control Node's outbound network capacity.

For this specific high-scale edge architecture, Ansible provides a native hybrid fallback: **`ansible-pull`**.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         ANSIBLE-PULL ARCHITECTURE                               │
│                                                                                 │
│   CENTRAL GIT REPO                                        EDGE / AUTO-SCALE VMs │
│   (Single Source of Truth)                                                      │
│   ┌───────────────────────┐                               ┌───────────────────┐ │
│   │ github.com/org/infra  │◄─── git clone via cron ───────│ Node 1 (runs      │ │
│   │ playbooks & roles     │◄─── (Every 15 minutes)        │ ansible-pull      │ │
│   └───────────────────────┘                               │ against localhost)│ │
│                                                           └───────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

In `ansible-pull` mode:
1. A cron job on the managed node runs `ansible-pull -U https://github.com/org/infra.git local.yml`.
2. The node clones the repository locally.
3. Ansible executes against `localhost` using `ansible_connection=local`, reversing the architecture to a decentralized pull model without requiring any third-party agents!

---

## 5. Summary & Hardening Checklist

1. **Enforce ED25519 Keys:** Do not use legacy 2048-bit RSA keys. Standardize on modern ED25519 SSH keys (`ssh-keygen -t ed25519`).
2. **Utilize Bastion Jump Hosts:** In production cloud environments, route connections through hardened jump hosts using OpenSSH `ProxyJump` directives rather than exposing managed nodes to the public internet.
3. **Always Enable SSH Pipelining:** Set `pipelining = True` in `ansible.cfg` to reduce SSH round-trips by up to 70%.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ansible Control Node vs Managed Nodes](./01-Ansible-Control-Node-vs-Managed-Nodes.md) | [Index](../../../README.md) | [03 - Ansible Execution Flow and Module Transport →](./03-Ansible-Execution-Flow-and-Module-Transport.md) |

