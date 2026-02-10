## Objective
Identify operating systems, services, software versions, frameworks, and
underlying technologies to guide focused enumeration and exploitation.

## When to Use
- After initial target discovery
- Before deep enumeration or exploitation
- Whenever a new service or application is identified
## 1️⃣ OS Fingerprinting
```bash
# OS detection via Nmap
nmap -O $ip

# Aggressive scan (includes OS, version, scripts)
nmap -A $ip
````

Look for:

* OS family (Linux, Windows)
* Kernel hints
* Device type (server, router, VM)

---

## 2️⃣ Service & Version Detection

```bash
# Service version detection
nmap -sV $ip

# Targeted service fingerprinting
nmap -p $port -sV $ip
```

Look for:

* Exact software versions
* Outdated or vulnerable services
* Service banners

---

## 3️⃣ Banner Grabbing (Manual)

```bash
# Netcat banner grab
nc -nv $ip $port

# Telnet banner grab
telnet $ip $port
```

Useful for:

* Custom services
* Legacy protocols
* Quick validation of Nmap results

---

## 4️⃣ Web Technology Fingerprinting

```bash
# Identify web technologies
whatweb -v $url
```

Identify:

* Web servers (Apache, Nginx, IIS)
* Frameworks (Laravel, Django, Spring)
* CMS (WordPress, Joomla, Drupal)

---

## 5️⃣ CMS & Framework Identification

```bash
# WordPress detection
wpscan --url $url --enumerate vp

# Generic CMS detection
whatweb $url
```

Look for:

* CMS version leaks
* Default paths
* Plugin exposure

---

## 6️⃣ HTTP Header Analysis

```bash
curl -I $url
```

Headers often reveal:

* Server software
* Framework hints
* Reverse proxies or load balancers

---

## 7️⃣ TLS / SSL Fingerprinting

```bash
openssl s_client -connect $ip:443
```

Look for:

* Certificate CN / SAN entries
* Internal hostnames
* Legacy crypto usage

---

## 8️⃣ SMB / Windows Fingerprinting

```bash
# SMB OS discovery
nmap --script smb-os-discovery -p 445 $ip

# SMB protocol versions
nmap --script smb-protocols -p 445 $ip
```

Identify:

* Windows version
* Domain or workgroup name
* SMB signing status

---

## 9️⃣ SNMP Fingerprinting (If Available)

```bash
# SNMP walk (default community)
snmpwalk -v2c -c public $ip
```

Can reveal:

* OS details
* Network interfaces
* Running services

---

## 🔟 Notes / Gotchas

* Fingerprinting is iterative — revisit after new discoveries
* Always validate automated results manually
* Versions ≠ vulnerabilities (confirm exploitability)
* Save fingerprints for exploit research and reporting
