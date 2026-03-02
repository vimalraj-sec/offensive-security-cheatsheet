## CHECKS

```md id="mgo3kp"
# Check if MongoDB is exposed (27017)
# Check MongoDB version
# Test for anonymous access
# List available databases
# Enumerate collections
# Dump collection data
# Search for credentials inside collections
# Check user roles and privileges
```
## INSTALL CLIENT

```bash id="v92k1h"
# Install MongoDB Client Tools
sudo apt install mongodb-clients -y

# Start Mongo Shell
mongo
```
## NMAP ENUMERATION

```bash id="0asf9x"
# Basic MongoDB Detection
nmap -p 27017 -sV $ip

# MongoDB Script Scan
sudo nmap -p 27017 --script mongodb-info $ip
```
## CONNECT / ACCESS

```bash id="jsh82m"
# Connect to Remote MongoDB
mongo $ip:27017

# Connect with Authentication
mongo $ip:27017 -u <user> -p <password> --authenticationDatabase admin
```
## DATABASE ENUMERATION

```bash id="mzl5h0"
# List Databases
show dbs

# Use Database
use <db>

# Show Collections
show collections
```
## COLLECTION ENUMERATION

```bash id="wq8lf3"
# Dump Collection
db.<collection>.find()

# Dump Collection (Readable Format)
db.<collection>.find().pretty()

# Count Records
db.<collection>.count()
```
## SEARCHING DATA

```bash id="fso7p1"
# Search for Specific Username
db.current.find({"username":"admin"})

# Search for Password Fields
db.<collection>.find({"password":{$exists:true}})
```
## USER & ROLE ENUMERATION

```bash id="r82jla"
# Show Users
use admin
db.getUsers()

# Show Current User
db.runCommand({connectionStatus : 1})
```
## NOTES

```md id="q8d29f"
- MongoDB runs on TCP 27017 (default)
- Older versions allowed unauthenticated access
- Databases and collections are case-sensitive
- Check for credential reuse inside documents
- Authentication database is often 'admin'
```
## REFERENCE

```md id="l2xk8e"
https://book.hacktricks.xyz/network-services-pentesting/27017-27018-mongodb
```

