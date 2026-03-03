## CHECKS

```md id="rs8c21"
# Check if rexec is exposed (512/TCP)
# Check if rlogin is exposed (513/TCP)
# Check if rsh is exposed (514/TCP)
# Identify plaintext authentication usage
# Check for host-based trust files (.rhosts, hosts.equiv)
# Verify if services are required
# Recommend migration to SSH
```
## NMAP ENUMERATION

```bash id="nm3rsh"
# Detect R services
nmap -p 512,513,514 -sV $ip

# Default Scripts
sudo nmap -p 512,513,514 --script default $ip
```
## INSTALL CLIENT (Lab / Testing)

```bash id="cl9rsh"
sudo apt install rsh-client -y
```
## LEGITIMATE LOGIN (If Authorized)

### 🔹 rlogin

```bash id="lg7rlo"
rlogin $ip -l <username>
```

### 🔹 rsh

```bash id="lg8rsh"
rsh -l <username> $ip
rsh $ip <command>
```
## TRUST FILE DISCOVERY

```bash id="tf6rsh"
# Search for host trust files
find / -name ".rhosts" 2>/dev/null
find / -name "hosts.equiv" 2>/dev/null
```
## BRUTFORCE
### 🔹 rsh
```md
hydra -L <Username_list> rsh://<Victim_IP> -v -V
```
### 🔹 rlogin
```md
hydra -l <username> -P <password_file> rlogin://<Victim-IP> -v -V
```
### 🔹 rexec
```md
hydra -l <username> -P <password_file> rexec://<Victim-IP> -v -V
```
## SECURITY RISKS

```md id="rk2rsh"
- Authentication sent in plaintext
- No encryption
- Vulnerable to spoofing attacks
- Relies on host-based trust
- Deprecated in modern systems
- High risk if exposed externally
```
## HARDENING / MITIGATION

```bash id="hd4rsh"
# Disable services
sudo systemctl disable rsh
sudo systemctl disable rlogin
sudo systemctl disable rexec

# Remove server package
sudo apt remove rsh-server -y

# Ensure SSH is enabled
sudo systemctl enable ssh
```
## NOTES

```md id="nt1rsh"
rexec  → TCP 512
rlogin → TCP 513
rsh    → TCP 514

These services are legacy Unix remote access mechanisms and should not be used in modern environments.
```
