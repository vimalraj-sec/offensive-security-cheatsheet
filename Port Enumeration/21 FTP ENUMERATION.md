## ENUMERATION CHECKLIST
```bash
- Check if you have anonymous access 
- Check if you can upload a file to trigger a webshell through the webapp  
- Check if you can download backup files to extract included passwords  
- Check the version of FTP for exploits
- Bruteforce Default credentials
```
## BASIC LOGIN
```bash
sudo ftp $ip  
sudo nc -nvv $ip 21  

# anonymous login check
sudo ftp ftp://anonymous:anonymous@$ip

# Change Modes - Passive
passive
# Change Modes - Active
quote port
# Change Modes - Binary
binary
# Change Modes - ASCII
ascii

# DEFAULT PASSWORDS
anonymous
admin
guest
```
## BRUTE FORCE - DEFAULT CREDENTIALS
```bash
sudo hydra -v -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt -f ftp://$ip

# Try special login cases bruteforce with user list
sudo hydra -L ./usernamelist -e nsr ftp://$ip
```
## DOWNLOADING FILE
```bash
ftp $ip 
PASSIVE
BINARY
get filename.txt

# Download all files
wget -m ftp://anonymous:anonymous@10.10.10.98 
wget -m --no-passive ftp://anonymous:anonymous@10.10.10.98 
```
## UPLOADING FILE
```bash
ftp $ip
PASSIVE
BINARY
put filename.txt
```
## BROWSER CONNECTION
```md
ftp://anonymous:anonymous@10.10.10.98
```
## NMAP FTP SCRIPTS
```bash
# VIEW NMAP SCRIPTS
ls -la /usr/share/nmap/scripts/*ftp*

# SCRIPTS
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-brute.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/tftp-enum.nse
```
## CONFIG FILES
```bash
/etc/ftpusers
/etc/pure-ftpd/pure-ftpd.conf
```