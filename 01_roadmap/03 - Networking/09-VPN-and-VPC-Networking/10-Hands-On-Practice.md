# 10 - Hands-On Practice: Setting Up a WireGuard VPN Tunnel

## Lab Scenario
Configure a secure point-to-point WireGuard VPN between Server A (`10.0.0.1/24`) and Server B (`10.0.0.2/24`).

---

## Lab Steps

### Step 1: Install WireGuard on Both Nodes
```bash
sudo apt update && sudo apt install -y wireguard
```

### Step 2: Generate Keypairs
On Server A:
```bash
wg genkey | tee serverA_private.key | wg pubkey > serverA_public.key
```
On Server B:
```bash
wg genkey | tee serverB_private.key | wg pubkey > serverB_public.key
```

### Step 3: Configure Server A (`/etc/wireguard/wg0.conf`)
```ini
[Interface]
Address = 10.100.0.1/24
ListenPort = 51820
PrivateKey = <Contents of serverA_private.key>

[Peer]
PublicKey = <Contents of serverB_public.key>
Endpoint = 203.0.113.20:51820
AllowedIPs = 10.100.0.2/32
PersistentKeepalive = 25
```

### Step 4: Configure Server B (`/etc/wireguard/wg0.conf`)
```ini
[Interface]
Address = 10.100.0.2/24
ListenPort = 51820
PrivateKey = <Contents of serverB_private.key>

[Peer]
PublicKey = <Contents of serverA_public.key>
Endpoint = 198.51.100.10:51820
AllowedIPs = 10.100.0.1/32
PersistentKeepalive = 25
```

### Step 5: Start WireGuard and Verify
```bash
sudo wg-quick up wg0
sudo wg show
ping 10.100.0.2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
