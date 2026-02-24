## CONNECT

```bash id="mysql1a9"
# Remote
mysql -h $ip -u root
mysql -h $ip -u root@localhost

# Remote with password
mysql -h $ip -u root -p

# Different port
mysql -h $ip -u root -p -P 13306

# Local (no password)
mysql -u root

# Local (prompt for password)
mysql -u root -p

# Skip SSL
sudo mysql -h $ip -u root@localhost -p --skip-ssl
```
## BASIC ENUMERATION (AFTER LOGIN)

```sql id="mysql2b7"
SHOW DATABASES;
USE <database>;
SHOW TABLES;
SELECT user,host FROM mysql.user;
SELECT user,authentication_string FROM mysql.user;
SELECT @@version;
SELECT database();
```
## NMAP ENUMERATION

```bash id="mysql3c4"
sudo nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 $ip
```
## METASPLOIT ENUMERATION

```bash id="mysql4d8"
use auxiliary/scanner/mysql/mysql_version
use auxiliary/scanner/mysql/mysql_authbypass_hashdump
use auxiliary/scanner/mysql/mysql_hashdump
use auxiliary/admin/mysql/mysql_enum
use auxiliary/scanner/mysql/mysql_schemadump
use exploit/windows/mysql/mysql_start_up
```
## PASSWORD UPDATE EXAMPLE

```sql id="mysql5e2"
UPDATE planning_user SET password='df5b909019c9b1659e86e0d6bf8da81d6fa3499e' WHERE user_id='ADM';
```
## NOTES

```md id="mysql6f1"
- Default port: 3306
- Common usernames: root, mysql, admin
- Check for empty passwords
- mysql.user table contains credential hashes
- Often reused credentials across services
```
