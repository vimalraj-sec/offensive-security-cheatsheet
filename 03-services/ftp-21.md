## 1️⃣ Enumeration Checklist

```bash
- Check for anonymous login
- Identify FTP service version
- Check read/write permissions
- Attempt file upload (webroot?)
- Download backups/config files
- Test default credentials
- Attempt credential brute-force (if allowed)
```

## 2️⃣ Basic Connectivity & Login

```bash
# Standard FTP login
ftp $ip

# Banner grabbing
nc -nvv $ip 21

# Anonymous login check
ftp ftp://anonymous:anonymous@$ip
```
### Mode Switching

```bash
# Passive mode (recommended)
passive

# Active mode
quote port

# Transfer modes
binary
ascii
```
## 3️⃣ Default Credentials

Common usernames/passwords:

```
anonymous
anonymous:anonymous
admin
guest
ftp
```

## 4️⃣ Bruteforce (If In Scope)

```bash
# Default credentials wordlist
hydra -v -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt -f ftp://$ip

# Username list with special cases (n=no pass, s=same as user, r=reverse)
hydra -L ./usernamelist -e nsr ftp://$ip
```

## 5️⃣ Downloading Files

```bash
ftp $ip
passive
binary
get filename.txt
```

### Download Entire FTP Share

```bash
wget -m ftp://anonymous:anonymous@$ip
wget -m --no-passive ftp://anonymous:anonymous@$ip
```

Look for:

* Backup files (.bak, .zip, .tar.gz)
* Config files
* Database dumps
* Credentials in plaintext
## 6️⃣ Uploading Files (If Write Access Allowed)

```bash
ftp $ip
passive
binary
put filename.txt
```

Check:

* Can uploaded files be accessed via web?
* Is the FTP directory mapped to `/var/www/`?
* Can you upload `.php`, `.asp`, `.jsp` files?

## 7️⃣ Browser-Based Access

```text
ftp://anonymous:anonymous@$ip
```

Useful for:

* Quick browsing
* Confirming public exposure

## 8️⃣ Nmap FTP Scripts

```bash
# Run common FTP scripts
nmap -p 21 --script=ftp-anon,ftp-syst,ftp-bounce $ip

# Full FTP script scan
nmap -p 21 --script=ftp* $ip
```

Common scripts:

```
ftp-anon.nse
ftp-bounce.nse
ftp-brute.nse
ftp-syst.nse
ftp-vsftpd-backdoor.nse
ftp-proftpd-backdoor.nse
ftp-vuln-cve2010-4221.nse
```

## 9️⃣ Configuration Files (If Local Access Obtained)

```bash
/etc/ftpusers
/etc/vsftpd.conf
/etc/pure-ftpd/pure-ftpd.conf
```

Check for:

* Disabled users
* Anonymous write enabled
* Chroot settings
* Passive port ranges
