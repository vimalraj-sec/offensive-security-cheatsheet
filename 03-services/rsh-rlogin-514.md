## CHECKS

```md
# Check if RSH/RLOGIN is exposed (514)
# Identify legacy remote access services
# Check for .rhosts trust relationships
# Verify if root login is permitted
# Check for host-based authentication
# Identify plaintext authentication risks
# Confirm service should be disabled (legacy protocol)
```
## NMAP ENUMERATION

```bash
# Basic Port Scan
nmap -p 514 $ip

# Service Detection
sudo nmap -sV -p 514 $ip

# RPC & R-Services Scripts
sudo nmap --script "r*" -p 514 $ip
```
## CLIENT INSTALLATION (For Testing in Lab)

```bash
sudo apt-get install rsh-client
```
## CONNECTION TEST (Authorized Environment Only)

```bash
rlogin <IP> -l <username>
```
## FILE DISCOVERY (Local System Audit)

```bash
# Search for trust files
find / -name .rhosts 2>/dev/null
```
## SECURITY NOTES

```md
- RSH/RLOGIN runs on TCP 514
- Authentication is transmitted in plaintext
- Uses host-based trust (.rhosts)
- Frequently allows passwordless login if misconfigured
- Considered deprecated and insecure
- Should be disabled and replaced with SSH
```
## HARDENING RECOMMENDATIONS

```md
- Disable rsh, rlogin, and rexec services
- Remove .rhosts files from user directories
- Block TCP 514 at firewall level
- Enforce SSH-only remote access
- Monitor for legacy service exposure
```