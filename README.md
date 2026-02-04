# 🛡️ Penetration Testing Cheatsheet ~ progressive 

## Description
- Practical penetration testing cheatsheet covering enumeration, exploitation, and privilege escalation  
- Includes commonly used offensive security techniques and workflows  
- Designed as a quick reference for labs, assessments, and real-world testing  
- Useful for penetration testers, security researchers, and learners  

## Intial Setup  
#### Create Working Directories
```bash
mkdir nmap fuzz www exploits dump scan; touch 01-machineip.txt 02-credentials.txt
```
#### Set Target Variables - Add the target IP to the file
```bash
nano 01-machineip.txt
```
#### Export commonly used environment variables
```bash
export ip=$(cat 01-machineip.txt);export url=http://$ip
```
#### Verify variables
```bash
echo $ip;echo -e;echo $url
```

## Port Scanning Using Nmap
#### Quick Port Scan
```bash
sudo nmap -Pn -p- -sV -sC -v -T5 --open --min-rate 1500 --max-rtt-timeout 500ms --max-retries 3 2>/dev/null -oN nmap/scan-script-version $ip
```
#### All Port Scan with Script & Version Scan - Oneliner
```bash
sudo nmap -p- -sC -sV -vv -oN nmap/scan-script-version $ip
```

#### More
- [Nmap](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/b0a677f96008929d0c91bff66cd7fe3e74c4908b/Initial%20Enumeration/QUICK%20NMAP%20CHEATSHEET.md)

## Port Enumeration
- [21 FTP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/21%20FTP%20ENUMERATION.md)
- [22 SSH Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/22%20SSH%20ENUMERATION.md)
- [23 Telnet Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/23%20TELENT%20ENUMERATION.md)
- [25 465 587 SMTP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/25%20465%20587%20SMTP%20ENUMERATION.md)
- [79 Finger Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/79%20FINGER%20ENUMERATION.md)
- [80 443 HTTP HTTPS Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e2c45c17d11edcb72c42381e35665e4912591ead/Port%20Enumeration/80%20443%20HTTP%20HTTPS%20ENUMERATION.md)
- [88 464 749 Kerberos Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/88%20464%20749%20KERBEROS%20ENUMERATION.md)
- [110 995 POP3 Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/110%20995%20POP3%20ENUMERATION.md)
- [135 593 RPC Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/135%20593%20RPC%20ENUMERATION.md)
- [139 445 SMB Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/139%20445%20SMB%20ENUMERATION.md)
- [143 993 IMAP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/143%20993%20IMAP%20ENUMERATION.md)
- [161 162 SNMP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/161%20162%20SNMP%20ENUMERATION.md)
- [389 636 3268 3268 LDAP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/389%20636%203268%203269%20LDAP%20ENUMERATION.md)
- [500 IPsec IKE VPN Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/500%20IPsec%20IKE%20VPN%20ENUMERATION.md)
- [512 REXEC Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/512%20RXEC%20%20ENUMERATION.md)
- [513 RLOGIN Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/513%20RLOGIN%20ENUMERATION.md)
- [514 RSH Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/514%20RSH%20ENUMERATION.md)
- [873 RSYSC Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/873%20RSYNC%20ENUMERATION.md)
- [1433 MSSQL Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/1433%20MSSQL%20ENUMERATION.md)
- [1521 Oracle DB Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/1521%20ORACLE%20DB%20-%20TNS%20ENUMERATION.md)
- [2049 NFS Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/2049%20NFS%20ENUMERATION.md)
- [3306 MySQL Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/3306%20MYSQL%20ENUMERATION.md)
- [3389 RDP Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/3389%20RDP%20ENUMERATION.md)
- [5432 5433 PostgreSQL Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/5432%205433%20POSTGRESQL.md)
- [5985 5986 WinRM Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/5985%205986%20WINRM%20ENUMERATION.md)
- [6379 Redis Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/6379%20REDIS%20ENUMERATION.md)
- [27017 27018 MongoDB Port](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5b4002df076ab31a9e9db4abaf8991fa76cd09d8/Port%20Enumeration/27017%2027018%20MONGODB.md)


