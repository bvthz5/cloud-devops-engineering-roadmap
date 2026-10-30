# 10 - Hands-On Practice: Hardening a Linux Host Firewall

## Lab Scenario
You are tasked with securing a production Ubuntu web server that hosts NGINX (80/443) and accepts administrative SSH traffic only from a corporate VPN IP (`198.51.100.25`).

---

## Lab Steps

### Step 1: Verify Current iptables Rules
```bash
sudo iptables -vnL --line-numbers
```

### Step 2: Establish the Golden Baseline Rules
```bash
# Allow loopback interface
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT

# Allow established and related connections
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Drop invalid packets immediately
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```

### Step 3: Allow Authorized Services
```bash
# Allow SSH only from corporate bastion
sudo iptables -A INPUT -p tcp -s 198.51.100.25 --dport 22 -j ACCEPT

# Allow public web traffic
sudo iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT
```

### Step 4: Set Default Policies to DROP
```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

### Step 5: Save Rules Permanently
```bash
sudo apt-get install -y iptables-persistent
sudo netfilter-persistent save
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
