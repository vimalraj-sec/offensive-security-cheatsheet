## CHECKS 
```md
# EXTENSIONS AND POSSBILE FILES
/index.html
/index.php
/index.asp
/index.aspx
/robots.txt
/sitemap.xml

# SOURCE CODE 
	- ctrl + u
	- Check for passwords/URLs/versions/... in comments of web app
# Brute force directories to find hidden content
# Check if specific CMS is used like WordPress and then use platform specific scanners
# CHECK VERSION NUMBERS FOR KNOWN EXPLOITS
	- Check changelog for version information
	- Estimate version based on copyright date (if not automatically adjusted)	
# LOGIN PORTALS  
 	- try the default credentials off the application  
 	- try usernames already seen throughout the application or in other services like SMTP  
 	- try SQL injection bypasses  
 	- try registering a new user  
 	- brute force with hydra, medusa, ...
# WAYS TO RCE
	- Check for file upload functionalities (if uploads are filtered, try alternative extensions)  
	- execute commands through SQLi  
	- Shellshock  
	- command injection  
	- trigger injected code through path traversal
```
## CURL + HTML2TEXT
```md
curl http://$ip | html2text
```
## HTTP METHODS
```md
sudo nmap --script http-methods --script-args http-methods.url-path='/website' $ip
curl -I -X OPTIONS http://192.168.100.10/test  
```
##  NIKTO
```bash
sudo nikto -h http://$ip 
```
## NMAP
```md
# LIST HTTP SCRIPTS
ls -la /usr/share/nmap/scripts/*http*

sudo nmap -sV -Pn --script=ssl-heartbleed,http-adobe-coldfusion-apsa1301.nse,http-apache-negotiation.nse,http-apache-server-status.nse,http-aspnet-debug.nse,http-auth-finder.nse,http-auth.nse,http-avaya-ipoffice-users.nse,http-awstatstotals-exec.nse,http-axis2-dir-traversal.nse,http-backup-finder.nse,http-barracuda-dir-traversal.nse,http-bigip-cookie.nse,http-brute.nse,http-cakephp-version.nse,http-cisco-anyconnect.nse,http-coldfusion-subzero.nse,http-comments-displayer.nse,http-config-backup.nse,http-cookie-flags.nse,http-cors.nse,http-cross-domain-policy.nse,http-csrf.nse,http-date.nse,http-default-accounts.nse,http-devframework.nse,http-dlink-backdoor.nse,http-dombased-xss.nse,http-domino-enum-passwords.nse,http-drupal-enum-users.nse,http-drupal-enum.nse,http-enum.nse,http-errors.nse,http-exif-spider.nse,http-feed.nse,http-fileupload-exploiter.nse,http-form-brute.nse,http-form-fuzzer.nse,http-frontpage-login.nse,http-git.nse,http-gitweb-projects-enum.nse,http-headers.nse,http-huawei-hg5xx-vuln.nse,http-iis-short-name-brute.nse,http-iis-webdav-vuln.nse,http-internal-ip-disclosure.nse,http-joomla-brute.nse,http-jsonp-detection.nse,http-litespeed-sourcecode-download.nse,http-ls.nse,http-majordomo2-dir-traversal.nse,http-mcmp.nse,http-method-tamper.nse,http-methods.nse,http-mobileversion-checker.nse,http-ntlm-info.nse,http-open-redirect.nse,http-passwd.nse,http-php-version.nse,http-phpmyadmin-dir-traversal.nse,http-phpself-xss.nse,http-proxy-brute.nse,http-put.nse,http-qnap-nas-info.nse,http-rfi-spider.nse,http-robots.txt.nse,http-security-headers.nse,http-server-header.nse,http-shellshock.nse,http-sitemap-generator.nse,http-sql-injection.nse,http-stored-xss.nse,http-svn-enum.nse,http-svn-info.nse,http-title.nse,http-tplink-dir-traversal.nse,http-trace.nse,http-traceroute.nse,http-trane-info.nse,http-unsafe-output-escaping.nse,http-useragent-tester.nse,http-userdir-enum.nse,http-vhosts.nse,http-vlcstreamer-ls.nse,http-vmware-path-vuln.nse,http-vuln-cve2006-3392.nse,http-vuln-cve2009-3960.nse,http-vuln-cve2010-0738.nse,http-vuln-cve2010-2861.nse,http-vuln-cve2011-3368.nse,http-vuln-cve2012-1823.nse,http-vuln-cve2013-0156.nse,http-vuln-cve2013-6786.nse,http-vuln-cve2013-7091.nse,http-vuln-cve2014-2126.nse,http-vuln-cve2014-2127.nse,http-vuln-cve2014-2128.nse,http-vuln-cve2014-3704.nse,http-vuln-cve2014-8877.nse,http-vuln-cve2015-1427.nse,http-vuln-cve2015-1635.nse,http-vuln-cve2017-1001000.nse,http-vuln-cve2017-5638.nse,http-vuln-cve2017-5689.nse,http-vuln-cve2017-8917.nse,http-vuln-misfortune-cookie.nse,http-vuln-wnr1000-creds.nse,http-waf-detect.nse,http-waf-fingerprint.nse,http-webdav-scan.nse,http-wordpress-brute.nse,http-wordpress-enum.nse,http-wordpress-users.nse,http-xssed.nse,membase-http-info.nse -p 80 $ip
```
## CMS SCANNERS
```bash
cmsmap [-f W] -F -d <URL>
wpscan --force update -e --url <URL>
joomscan --ec -u <URL>
joomlavs.rb #https://github.com/rastating/joomlavs
```


