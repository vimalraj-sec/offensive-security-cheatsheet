```bash
# Retrieve HTTP/HTTPS Headers
sudo curl -I $url
sudo curl -k -I $url
sudo curl --insecure -I $url

# Retrieve HTTP Headers and Follow Redirects
sudo curl -IL $url

# HTTP Methods Check
sudo curl -X OPTIONS $url -i

# WhatWeb - Verbose Output
sudo whatweb -v $url

# Retrieve Full Content in Text Format
sudo curl -vs $url | html2text | less

# Save Content to File in Text Format
sudo curl -vs $url | html2text > output.txt

# Extract All Links (without query parameters) and Sort
sudo curl -s $url | grep -oP 'href="\K[^"]+' | grep -v '\?' | sort -u | less

# Check for SSL/TLS Information
openssl s_client -connect $ip:443

# CHECK FOR COMMON FILES (USING A LOOP)
for file in robots.txt sitemap.xml index.html index.php index.asp index.aspx; do echo -e "\n\033[1;36m=== $url/\033[1;33m$file\033[1;36m ===\033[0m"; curl -s "$url/$file" | cat; done
```
## NIKTO 
```bash
sudo nikto -h $url
```
## NMAP
```bash
sudo nmap -p 80,443 --script=http-enum -oN nmap/script-http-enum $ip
```
## BROWSER EXTENSIONS
```bash
- wappalyzer
- builtwith
```