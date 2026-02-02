## CONNECT
```bash
# Remote 
mysql -h $ip -u root
mysql -h $ip -u root@localhost

# Different port
mysql -h $ip -u root -p -P 13306 
# Local
# Connect to root without password
mysql -u root 
# A password will be asked (check someone)
mysql -u root -p 

# Skip SSL
sudo mysql -h $ip -u root@localhost -p --skip-ssl
```
## NMAP
```bash
sudo nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 $ip
```
## MSFCONSOLE - METASPLOIT
```bash
msf> use auxiliary/scanner/mysql/mysql_version
msf> use auxiliary/scanner/mysql/mysql_authbypass_hashdump
msf> use auxiliary/scanner/mysql/mysql_hashdump #Creds
msf> use auxiliary/admin/mysql/mysql_enum #Creds
msf> use auxiliary/scanner/mysql/mysql_schemadump #Creds 
msf> use exploit/windows/mysql/mysql_start_up #Execute commands Windows, Creds
```
## UPDATE PASSWORD EXAMPLE
```bash
UPDATE planning_user SET password='df5b909019c9b1659e86e0d6bf8da81d6fa3499e' WHERE user_id='ADM';
```