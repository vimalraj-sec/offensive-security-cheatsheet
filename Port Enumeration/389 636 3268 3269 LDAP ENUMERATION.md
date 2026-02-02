## NMAP 
```bash
# USAGE
sudo nmap -p 389 --script ldap-search $ip

# Run Scripts Scan
sudo nmap -n -sV --script "ldap* and not brute" -p 389 $ip

sudo nmap -p 389 --script ldap-novell-getpass.nse,ldap-rootdse.nse,ldap-search.nse $ip
```
## LDAPSEARCH
```bash
# Basic
sudo ldapsearch -x -H ldap://$ip

# Simple Authentication
sudo ldapsearch -x -H ldap://$ip -b "dc=ENTERPRISE,dc=THM"

# Get All Attributes
sudo ldapsearch -x -H ldap://$ip -b "dc=ENTERPRISE,dc=THM" "*"

# Basic LDAP Search for a base-level
sudo ldapsearch -h <target_ip> -x -s base

# Get Naming Contexts
sudo ldapsearch -x -H ldap://$ip -s base namingcontexts
```
## LDAPDOMAINDUMP
```bash
ldapdomaindump $ip

# Create a folder - as it generates more files

# Dump Information
sudo ldapdomaindump ldaps://192.168.100.12 -u 'OSCP.EXAM\clarkkent' -p 'Password1'

# firefox to view the info
```