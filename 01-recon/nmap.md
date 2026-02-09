## QUICK
```bash
sudo nmap -Pn -p- -sV -sC -v -T5 --open --min-rate 1500 --max-rtt-timeout 500ms --max-retries 3 2>/dev/null -oN nmap/scan-script-version $ip
```
---
## COPY NMAP OUTPUT
```bash
cat nmap/scan-script-version | pbcopy
```
```bash
grep -E '^[0-9]+/tcp.*open' nmap/scan-script-version | pbcopy
```
```bash
cat nmap/scan-script-version | grep -v "adjust_timeouts2:" | pbcopy
```
## NMAP UDP  SCAN
```bash
sudo nmap -sU -sV --top-ports 20 -oN nmap/scan-udp-top20 $ip
```
```bash
sudo nmap -sU -sV -T4 --top-ports 20 -oN nmap/scan-udp-top20 $ip
```
## NMAP ALL PORT SCRIPT VERSION SCAN - ONELINERS
 ```bash
sudo nmap -p- -sC -sV -vv -oN nmap/scan-script-version $ip
```
```bash
sudo nmap -Pn -p- -sC -sV -vv -oN nmap/scan-script-version $ip
```
## COPY PORTS
```bash
ports=$(cat nmap/scan-allports| grep tcp | awk '{print $1}' | cut -d "/" -f 1| awk -vORS="," '{ print $1 }'| sed 's/,$//' |sed 's/Not,//')
```
## NMAP NSE DEFAULT VULN SCRIPT SCAN
```bash
sudo nmap -p $ports --script "vuln" -oN nmap/scan-script-vuln $ip
sudo nmap -p $ports -sV --script "vuln" -oN nmap/scan-script-vuln $ip
```
---
# METHOD II
## QUICK
```bash
sudo nmap -p- -Pn -v -T5 --min-rate 1500 --max-rtt-timeout 500ms --max-retries 3 --open -oN nmap/scan-allports $ip
```
```bash
sudo nmap -p- -Pn -T5 -vvv --min-rate 10000 -oN nmap/scan-allports $ip
```
```bash
sudo nmap -p- -Pn -T5 -vvv -oN nmap/scan-allports $ip
```
## RUSTSCAN
```bash
rustscan -a $ip -r 1-65535 -u 5000 -g
```
## Run Script and Version Scan on ports
```bash
sudo nmap -p $ports -sC -sV -vv -oN nmap/scan-script-version $ip
```