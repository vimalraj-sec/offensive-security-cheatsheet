## BANNER GRABBING
```md
nc -nv $ip 110
openssl s_client -connect $ip:110 -starttls pop3
openssl s_client -connect $ip:995 -crlf -quiet
```
## COMMANDS TO CHECK 
```md
telnet $ip 110
      
# POP commands:
  USER uid           Log in as "uid"
  PASS password      Substitue "password" for your actual password
  STAT               List number of messages, total mailbox size
  LIST               List messages and sizes
  RETR n             Show message n
  DELE n             Mark message n for deletion
  RSET               Undo any changes
  QUIT               Logout (expunges messages if no RSET)
  TOP msg n          Show first n lines of message number msg
  CAPA               Get capabilities
```
## NMAP
```md
sudo nmap --script "pop3-capabilities or pop3-ntlm-info" -sV -p <PORT> $ip #All are default scripts
```
## EXAMPLE
```bash
    USER <username> identifies the user
    PASS <password> provides the user’s password
    STAT requests the number of messages and total size
    LIST lists all messages and their sizes
    RETR <message_number> retrieves the specified message
    DELE <message_number> marks a message for deletion
    QUIT ends the POP3 session applying changes, such as deletions
```
## RECEIVE MAIL - EXAMPLE
```bash
user@TryHackMe$ telnet 10.10.212.122 110
Trying 10.10.212.122...
Connected to 10.10.212.122.
Escape character is '^]'.
+OK [XCLIENT] Dovecot (Ubuntu) ready.
AUTH
+OK
PLAIN
.
USER strategos
+OK
PASS 
+OK Logged in.
STAT
+OK 3 1264
LIST
+OK 3 messages:
1 407
2 412
3 445
.
RETR 3
+OK 445 octets
Return-path: <user@client.thm>
Envelope-to: strategos@server.thm
Delivery-date: Thu, 27 Jun 2024 16:19:35 +0000
Received: from [10.11.81.126] (helo=client.thm)
        by example.thm with smtp (Exim 4.95)
        (envelope-from <user@client.thm>)
        id 1sMrpq-0001Ah-UT
        for strategos@server.thm;
        Thu, 27 Jun 2024 16:19:35 +0000
From: user@client.thm
To: strategos@server.thm
Subject: Telnet email

Hello. I am using telnet to send you an email!
.
QUIT
+OK Logging out.
Connection closed by foreign host.
```