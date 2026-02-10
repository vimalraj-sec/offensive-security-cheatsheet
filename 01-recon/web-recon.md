# HTTP Enumeration (Web Recon)

## Objective
Identify web technologies, HTTP behavior, misconfigurations, and exposed files
before performing fuzzing or exploitation.

## When to Use
- Port 80 / 443 discovered during reconnaissance
- Any HTTP-based service (Apache, Nginx, IIS, Tomcat, etc.)

## 1️⃣ HTTP Header & Response Analysis

```bash
# Retrieve HTTP/HTTPS headers
curl -I $url
curl -k -I $url
curl --insecure -I $url

# Follow redirects
curl -IL $url

```
Look for:
- Server type
- Framework hints
- Redirect chains
- Security headers

## 2️⃣ HTTP Methods Enumeration
```bash
# Check allowed HTTP methods
curl -X OPTIONS $url -i
```
Look for:
- PUT / DELETE enabled
- WebDAV indicators

## 3️⃣ Technology Fingerprinting
```bash
# Identify technologies
whatweb -v $url
```
Browser Extensions (passive):
- Wappalyzer
- BuiltWith
## 4️⃣ Content Inspection (Manual)
```bash
# Retrieve full page content in readable format
curl -vs $url | html2text | less

# Save content to file
curl -vs $url | html2text > output.txt
```
Use this to:
- Find hidden endpoints
- Identify parameters
- Detect comments or API hints
## 5️⃣ Link Extraction
```bash
# Extract links (excluding query parameters)
curl -s $url | grep -oP 'href="\K[^"]+' | grep -v '\?' | sort -u | less
```
## 6️⃣ TLS / SSL Inspection
```bash
# Check TLS configuration
openssl s_client -connect $ip:443
```
Look for:
- Certificate names
- Internal hostnames
- Expired or weak configs
## 7️⃣ Common File Checks (Manual)
```bash
for file in robots.txt sitemap.xml index.html index.php index.asp index.aspx; do
  echo -e "\n=== $url/$file ==="
  curl -s "$url/$file"
done
```
These often reveal:
- Hidden paths
- Admin panels
- Backup logic
## 8️⃣ Automated Web Enumeration (Light)
```bash
nikto -h $url
```
Use Nikto for:
- Quick misconfiguration checks
- Outdated server findings
⚠️ Do not rely on Nikto alone.

## 9️⃣ Nmap HTTP Scripts
```bash
nmap -p 80,443 --script=http-enum -oN nmap/script-http-enum $ip
```
Useful for:
- Quick content discovery
- Baseline enumeration
