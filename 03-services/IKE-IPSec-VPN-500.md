## 🔎 Nmap – Identify IKE Service

```bash
sudo nmap -sU -p 500 $ip -oN nmap/scan-port500
```
## 🛰️ IKE Scan – Detect VPN & Transforms

```bash
sudo ike-scan -M $ip
```
### 🔍 Security Association (SA) Example

```
SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration(4)=0x00007080)
```

### 📌 Important Fields

- `Enc` → Encryption algorithm
    
- `Hash` → Hashing algorithm
    
- `Group` → Diffie-Hellman group
    
- `Auth=PSK` → Uses Pre-Shared Key authentication
    
- `LifeDuration` → SA lifetime
    
### 📊 IKE Scan Result Meanings

- `0 handshake / 0 notify` → Not an IPsec gateway
    
- `1 handshake / 0 notify` → Valid IPsec service, acceptable transform
    
- `0 handshake / 1 notify` → IPsec device present, transform rejected
    
# 🔐 strongSwan – IPsec Tunnel Setup

## 📦 Install

```bash
sudo apt install strongswan -y
```
## 📝 Configure PSK

```bash
sudo nano /etc/ipsec.secrets
```

Add your pre-shared key inside this file.
## 📝 Configure Tunnel

```bash
sudo nano /etc/ipsec.conf
```

### Example Configuration

```
conn Conceal
    type=transport
    keyexchange=ikev1
    left=10.10.14.47
    leftprotoport=tcp
    right=10.129.239.79
    rightprotoport=tcp
    authby=psk
    fragmentation=yes
    esp=3des-sha1
    ike=3des-sha1-modp1024
    ikelifetime=8h
    auto=start
```
## 🌐 Adjust MTU (If Needed)

```bash
ifconfig tun0 mtu 1000
```
## 🚀 Start IPsec Tunnel

```bash
sudo ipsec start --nofork
```
## 🔎 Scan Through Tunnel

```bash
sudo nmap -sT -p- $ip
```
