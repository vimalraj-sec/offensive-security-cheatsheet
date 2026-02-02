## NETEXEC
```bash
# Check Shares
sudo nxc smb $ip --shares

# Check Shares Anonymous Authentication
sudo nxc smb $ip --shares -u 'anonymous' -p ''
sudo nxc smb $ip --shares -u 'guest' -p ''

# Spider Share
sudo nxc smb $ip -u 'anonymous' -p '' --spider SHARENAME --regex .

# Download File from share
sudo nxc smb $ip -u 'anonymous' -p '' --share SHARENAME --get-file /remote/path/to/file /local/path/to/save

# Rid-brute Force - Anonymous
sudo nxc smb $ip -u 'anonymous' -p '' --rid-brute | tee raw-rid
grep "SidTypeUser" raw-rid | awk -F'\\\\' '{print $2}' | awk '{print $1}' > usernames

# Brute force Creds
sudo nxc smb $ip --shares -u ./users -p ./passwords --continue-on-success
sudo nxc smb $ip --shares -u 'anonymous' -p '' --local-auth
```
## SMBCLIENT
```bash
# List Shares
sudo smbclient -L $ip
```
## MOUNT SMB SHARES
```bash
# Mount a smb share to a folder
sudo mount -t cifs -o 'user=brucewayne' //$ip/SHARENAME /mnt/share

# Domain Share Mount
sudo mount -t cifs -o 'user=brucewayne,domain=GOTHAM' //$ip/SHARENAME /mnt/share

# LIST ALL
ls -laR /mnt/sharee
```
## VIEW SHARE PERMISSIONS
```bash
# Check Permission for single folder
sudo smbcacls --no-pass //$ip/ShareName Folder

# Check Permission for all folders on the mounted share
for i in $(ls /mnt/share); do echo "[*] Checking: $i"; sudo smbcacls --no-pass //$ip/ShareName "$i"; echo; done
```
# MORE
## NETEXEC
```bash
# CHECK SHARES
sudo nxc smb $ip --shares

# CHECK SHARES ANONYMOUS AUTHENTICATION
sudo nxc smb $ip --shares -u 'anonymous' -p ''
sudo nxc smb $ip --shares -u 'guest' -p ''

# RID-BRUTEFORCE - Anonymous
sudo nxc smb $ip -u 'anonymous' -p '' --rid-brute | tee raw-rid
cat raw-rid | cut -d "\\" -f 2 | grep SidTypeUser | cut -d  " " -f 1 > usernames

# LIST USERS
sudo nxc smb $ip -u 'anonymous' -p '' --users

# SPIDER SHARE
sudo nxc smb $ip -u 'anonymous' -p '' --spider SHARENAME --regex .

# Download File from share
sudo nxc smb $ip -u 'anonymous' -p '' --share SHARENAME --get-file /remote/path/to/file /local/path/to/save
sudo nxc smb $ip -u 't-skid' -p 'tj072889*' --share 'NETLOGON' --get-file 'ResetPassword.vbs' './ResetPassword.vbs'

# DUMP Hashes
sudo nxc smb $ip -u 'administrator' -p 'password123' --sam

# Logged on Users
sudo nxc smb $ip -u 'administrator' -p 'password123' --loggedon-users

# NTDS
sudo nxc smb $ip -u 'administrator' -p 'password123' --ntds

# lsa
sudo nxc smb $ip -u 'administrator' -p 'password123' --lsa

# CHECK SHARES NULL AUTHENTICATION
sudo nxc smb $ip --shares -u '' -p ''

# CHECK AUTHENTICATION WITH VALID USERNAME AND PASSWORD
sudo nxc smb $ip --shares -u 'user' -p 'password'

# DIFFERENT PORT
sudo nxc smb $ip --port 36445

# --local-auth
sudo nxc smb $ip --shares -u 'user' -p 'password' --local-auth
sudo nxc smb $ip --shares -u '' -p '' --local-auth

# Password brute
sudo nxc smb $ip --shares -u 'anonymous' -p '' --local-auth
sudo nxc smb $ip --shares -u 'anonymous' -p '' --continue-on-success
```
## SMBCLIENT
```bash
# List shares on a machine using NULL Session
sudo smbclient -L $ip
sudo smbclient -L //$ip/
sudo smbclient -L \\\\$ip\\
# Access Share
sudo smbclient //$ip/SHARE/

# Windows
sudo smbclient -L $ip -U Administrator

# DOWNLOAD ALL THE CONTENTS OF THE SHARE - after accessing the share
smb: \> prompt off
smb: \> recurse on
smb: \> mget *

# NULL AUTHENTICATION
sudo smbclient -U '' -L //$ip/
sudo smbclient -U '.' -L //$ip/
sudo smbclient \\\\$ip\\[share name]

# List share with username and password
sudo smbclient -L //$ip/ -U username%password

# Access share with username and password
sudo smbclient //$ip/share$ -U username%password
```
## CRACKMAPEXEC
```bash
# INTIAL SCAN
sudo crackmapexec smb $ip
# CHECK BANNER
sudo crackmapexec smb $ip
# CHECK SHARES
sudo crackmapexec smb $ip --shares
# CHECK SHARES NULL AUTHENTICATION
sudo crackmapexec smb $ip --shares -u '' -p ''
# CHECK SHARES ANONYMOUS AUTHENTICATION
sudo crackmapexec smb $ip --shares -u 'anonymous' -p ''
# CHECK AUTHENTICATION WITH VALID USERNAME AND PASSWORD
sudo crackmapexec smb $ip --shares -u 'user' -p 'password'

# --local-auth
sudo crackmapexec smb $ip --shares -u 'user' -p 'password' --local-auth
```
## NMAP
```bash
# SHARES
sudo nmap -p 139,445 --script=smb-enum-shares.nse,smb-enum-users.nse $ip

# SAFE SCAN
sudo nmap --script "safe or smb-enum-*" -p 445 $ip -oN nmap/smb-safe-script $ip

# ENUMERATE SHARES
sudo nmap --script smb-enum-shares -p 139,445 -oN nmap/smb-enum-shares $ip

# SMB VULN SCAN
sudo nmap --script smb-vuln* -p 139,445 -oN nmap/smb-vuln-script $ip
```
## SMBMAP
```bash
# INITIAL SCAN
sudo smbmap -H $ip

# DEPTH SCAN
sudo smbmap -H $ip -R --depth 5

# WITH PORTS
sudo smbmap -H $ip -R --depth 5 -P 139 

# WITH CREDENTIALS
sudo smbmap -H $ip -d [domain] -u [user] -p [password]

# DOWNLOAD CONTENTS FROM SHARE
smbmap -u null -p null -H <IP> -s SHARENAME$ -R
```
## ENUM4LINUX
```bash
# enum4linux - General enumeration - anonymous session 
sudo enum4linux -a $ip | tee scan/enum4linux_output.txt
 
# enum4linux - General enumeration - authenticated session
sudo enum4linux -u username -p password -a $ip
 
# enum4linux - Users enumeration
sudo enum4linux -u username -p password -U $ip
 
# enum4linux - Group and members enumeration 
sudo enum4linux -u username -p password -G $ip
 
# enum4linux - Password policy
sudo enum4linux -u username -p password -P $ip
```
## ENUM4LINUX-NG
```bash
sudo enum4linux-ng $ip | tee scan/enum4linux-ng_output.txt
sudo enum4linux-ng $ip -oY scan/enum4linux-ng-scan
```
## MOUNT SHARE FOLDER
```bash
sudo mkdir /mnt/share

sudo mount -t cifs //$ip/share /mnt/share
sudo mount -t cifs -o "username=user,password=password" //$ip/share /mnt/share
```
## SMBPASSWD
```bash
# Need Old password to change password
sudo smbpasswd -U tlavel -r $ip
```
## ACCESS WINDOWS SHARE VIA PORT FORWARDING PORT 
```bash
# EDIT /etc/samba/smb.conf
sudo nano /etc/samba/smb.conf
min protocol = SMB2
sudo /etc/init.d/smbd restart
```