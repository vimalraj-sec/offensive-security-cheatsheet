# 🛡️ Penetration Testing Cheatsheet (Progressive)

A practical, step-by-step penetration testing cheatsheet covering **enumeration, exploitation, and privilege escalation**.

### 🎯 Purpose
- Offensive Security Certification preparation
- Labs: Hack The Box (HTB), TryHackMe (THM), Proving Grounds (PG)
- Real-world penetration testing engagements
- Quick reference during assessments

## 📌 Penetration Testing Workflow
- Scope & Target setup  
- Reconnaissance  
- Network & Service enumeration  
- Web application enumeration  
- Privilege escalation
- Post-exploitation

Each stage below links directly to focused cheatsheets.

## 📁 Repository Structure & Navigation

### ⚙️ 00 – Scope & Target setup
Prepare the environment and define the target.
- 👉 [Environment & Target Setup](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/9e7ac56e50dc4b8fdceb45104bbb791b5803ee00/00-setup/environment-setup.md)

### 🔍 01 – Reconnaissance
Identify open ports, running services, and exposed technologies.
- 👉 [Nmap Scanning](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/e4b7b118d92676a4dd5856d257ef82319ddd91f0/01-recon/nmap.md)
- 👉 [Web Application Recon](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/be3e4cd70f562c7acb172cd2cfe8a7ea890aafc2/01-recon/web-recon.md)
- 👉 [Subdomains](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/82f88548eb0abf9f08b36c22c603844f92ffe75f/01-recon/subdomains.md)
- 👉 [Technology Fingerprinting](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/fe374d57b32fc1bc067d76dfecb8b3c45058a8fb/01-recon/technology-fingerprinting.md)

### 🌐 02 – Web Application Enumeration
Enumerate web applications running on discovered services.
- 👉 [Directory & File Fuzzing](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/b034222748c882366956b3346c37c07a1eac4e53/02-web/directory-file-fuzzing.md)

### 🔌 03 – Service Enumeration
Enumerate non-web services for misconfigurations and weak credentials.
- 👉 [Port Knocking](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5eeaa4d5292fbd7d368037f44c9f1c5ca540c672/03-services/port-knocking.md)
- 👉 [FTP Enumeration (Port 21)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/0abae55ad193cf6134aba4ee0cac7a1b1f5c0946/03-services/ftp-21.md)
- 👉 [SSH Enumeration (Port 22)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/1f748b4eb1a44e50246e8989773519c0a5bf6bdc/03-services/ssh-22.md)
- 👉 [Telnet Enumeration (Port 23)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/ef1eac510fd0a6b10b3debb4d07f4df59f8cff10/03-services/telnet-23.md)
- 👉 [SMTP Enumeration (Port 25 465 587)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/5a08b7ab54c590d416a8553aad27fb894ab405a1/03-services/smtp-25.md)
- 👉 [Finger Enumeration (Port 79)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/ef1eac510fd0a6b10b3debb4d07f4df59f8cff10/03-services/finger-79.md)
- 👉 [POP3 Enumeration (Port 110 995)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/4e2f4d8d1d34dac5449e19c0124dc6fa2873cfa9/03-services/pop3-110-995.md)
- 👉 [SMB Enumeration (Port 139 445)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/26a42a477bcbba980c2307752e9bfcf51832929e/03-services/smb-139-445.md)
- 👉 [IMAP Enumeration (Port 143 993)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/8de5ff50f56f819779bdfd83ed80074f84fad015/03-services/imap-143-993.md)
- 👉 [LDAP Enumeration (Port 389 636)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/8a1814ec7e4ea99de91d854e09c113baabc34190/03-services/ldap-389-636.md)
- 👉 [RSH RLOGIN Enumeration (Port 514)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/13cfea908e78524e94cfc76c992b14b1486d64ca/03-services/rsh-rlogin-514.md)
- 👉 [IKE IPSec VPN Enumeration (Port 500)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/0aba9be2cc04d47a6ecad09fa622b1dd26ddc6dc/03-services/IKE-IPSec-VPN-500.md)
- 👉 [MSSQL Enumeration (Port 1433)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/327cca31058977ea139d1cb7f97c0f392e26e533/03-services/mssql-1433.md)
- 👉 [NFS Enumeration (Port 2049)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/c3c43553a92be280c745486343797155718be4aa/03-services/nfs-2049.md)
- 👉 [MySQL Enumeration (Port 3306)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/6c904bfd50a993be0d2e020f78036961eaf6567f/03-services/mysql-3306.md)
- 👉 [RDP Enumeration (Port 3389)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/9d6f1a520b3885af27bea371ad30d7ad6b28d46d/03-services/rdp-3389.md)
- 👉 [PostgreSQL Enumeration (Port 5432)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/9df57091cff45e9606db6e6e1eeb7c29d6d8ac87/03-services/postgresql-5432-5433.md)
- 👉 [Redis Enumeration (Port 6379)](https://github.com/vimalraj-sec/offensive-security-cheatsheet/blob/11d9830fead7294d650070ef111de147df132ca3/03-services/redis-6379.md)

# ~ To be Updated
### 🚀 04 – Exploitation
Exploit identified vulnerabilities to gain initial access.

### 🧠 05 – Privilege Escalation
Escalate privileges after initial access.

### 🧠 05 – Privilege Escalation
Escalate privileges after initial access.

### 📚 Resources
Helpful references and wordlists.



