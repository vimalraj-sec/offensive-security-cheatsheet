## ONESIXTYONE - BRUTEFORCE
```bash
sudo onesixtyone $ip -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt
```
## SNMP-WALK
```bash
# Enumerate MIB Tree using community 
sudo snmpwalk -c <community> $ip
sudo snmpwalk -c <community> -v1 $ip
sudo snmpwalk -c <community> -v2c $ip

# Windows SNMP Enumeration
# Enumerating the Entire MIB Tree 
snmpwalk -c public -v2c $ip
snmpwalk -c public -v1 -t 10 10.11.1.14

# Enumerating Windows Users
snmpwalk -c public -v1 10.11.1.14 1.3.6.1.4.1.77.1.2.25

# Enumerating Running Windows Processes
snmpwalk -c public -v1 10.11.1.73 1.3.6.1.2.1.25.4.2.1.2

# Enumerating Open TCP Ports
snmpwalk -c public -v1 10.11.1.14 1.3.6.1.2.1.6.13.1.3

# Enumerating Installed Software
snmpwalk -c public -v1 10.11.1.50 1.3.6.1.2.1.25.6.3.1.2

# Windows SNMP MIB values
1.3.6.1.2.1.25.1.6.0 System Processes
1.3.6.1.2.1.25.4.2.1.2 Running Programs
1.3.6.1.2.1.25.4.2.1.4 Processes Path
1.3.6.1.2.1.25.2.3.1.4 Storage Units
1.3.6.1.2.1.25.6.3.1.2 Software Name
1.3.6.1.4.1.77.1.2.25 User Accounts
1.3.6.1.2.1.6.13.1.3 TCP Local Ports
```
## SNMP-CHECK
```bash
# SYNTAX
sudo snmp-check $ip

# Enumerate community strings
sudo snmp-check -c <community> $ip
# Version
sudo snmp-check -c <community> -v1 $ip
sudo snmp-check -c <community> -v2c $ip
sudo snmp-check -c <community> -v2u $ip
sudo snmp-check -c <community> -v3 $ip
```
## NMAP
```bash
# SNMP Nmap Scan 
sudo nmap -sU --open -p 161 $ip
# Nmap Script Scan
sudo nmap -sU --open -p 161 -sV -sC $ip -oN nmap/scan-snmp-scriptscan
```
