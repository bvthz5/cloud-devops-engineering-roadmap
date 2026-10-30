# 10 - Hands-On Practice: Setting Up Control Node & Managed Node

## Lab Goal
Configure a local Ansible environment with 1 Control Node and 2 Managed Nodes.

---

## Step 1: Install `ansible-core` on Control Node
```bash
sudo apt update
sudo apt install -y python3-pip python3-venv
python3 -m venv ~/ansible-venv
source ~/ansible-venv/bin/activate
pip install ansible-core
```

---

## Step 2: Create Project Directory Structure
```bash
mkdir -p ~/ansible-lab/{inventory,group_vars}
cd ~/ansible-lab
```

Create `ansible.cfg`:
```ini
[defaults]
inventory = ./inventory/hosts.ini
host_key_checking = False
remote_user = ubuntu
```

Create `inventory/hosts.ini`:
```ini
[web]
web01 ansible_host=127.0.0.1 ansible_port=2222 ansible_connection=local
```

---

## Step 3: Test Connectivity
```bash
ansible all -m ping
```

Expected Output:
```json
web01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
