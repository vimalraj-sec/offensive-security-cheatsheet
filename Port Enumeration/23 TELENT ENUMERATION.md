##  BASIC LOGIN
```bash
sudo nc -vn $ip 23

telnet $ip 23
```
## NMAP
```bash
# VIEW SCRIPTS
ls -la /usr/share/nmap/scripts/*telnet*

# TELNET SCRIPTS
/usr/share/nmap/scripts/telnet-brute.nse
/usr/share/nmap/scripts/telnet-encryption.nse
/usr/share/nmap/scripts/telnet-ntlm-info.nse

sudo nmap -n -sV -Pn --script "*telnet* and safe" -p 23 $ip
```
## CONFIG FILES
```bash
/etc/inetd.conf
/etc/xinetd.d/telnet
/etc/xinetd.d/stelnet
```