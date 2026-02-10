## Basic DNS subdomain brute force
```bash
sudo ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://FUZZ.cmess.thm -t 40 -fc 404

# Using wfuzz
wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hc 404 -t 40 http://FUZZ.cmess.thm
```
## Virtual Host (Host Header) Fuzzing
```bash
# ffuf vhost fuzzing
sudo ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.cmess.thm" -u http://cmess.thm/ -t 40 -fc 404 | tee fuzz/ffuf-subdomain

# wfuzz vhost fuzzing
sudo wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.cmess.thm" -u http://cmess.thm -t 40 --hc 404 | tee fuzz/wfuzz-subdomain
```
## Validate & Resolve Discovered Subdomains
```bash
# Add to /etc/hosts if needed
sudo nano /etc/hosts

# Example adding hosts to /etc/hosts
10.10.10.10 admin.cmess.thm
10.10.10.10 dev.cmess.thm
```
