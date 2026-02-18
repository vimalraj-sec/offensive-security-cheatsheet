## CHECKS

```md
# Check for weak / default credentials
# Check for cleartext authentication
# Check service version for known exploits
# Attempt brute force if necessary
```
## BASIC LOGIN

```bash
# Netcat
nc -vn $ip 23

# Telnet Client
telnet $ip 23
```
## NMAP ENUMERATION

```bash
# View Telnet Scripts
ls -la /usr/share/nmap/scripts/*telnet*

# Run Safe Telnet Scripts
sudo nmap -n -sV -Pn --script "*telnet* and safe" -p 23 $ip
```

Common Telnet NSE scripts:

```bash
telnet-brute.nse
telnet-encryption.nse
telnet-ntlm-info.nse
```
## BRUTE FORCE (If Needed)

```bash
# Hydra
sudo hydra -l user -P /usr/share/wordlists/rockyou.txt telnet://$ip

# Multiple Users
sudo hydra -L users.txt -P passwords.txt -t 16 telnet://$ip
```
## CONFIG FILES

```bash
/etc/inetd.conf
/etc/xinetd.d/telnet
/etc/xinetd.d/stelnet
```
## NOTES

```md
- Telnet transmits credentials in cleartext
- Often replaced by SSH
- High-value target if exposed externally
- Always check for reused SSH credentials
```