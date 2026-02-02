## RPCCLIENT
```md
# RPCCLIENT
sudo rpcclient -U "" $ip

# CHECK NULL SESSIONS
sudo rpcclient -U "" -N $ip

# DOCUMENTATION
https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html
```
## ACTIVE DIRECTORY ENUMERATION  - RPCCLIENT
```md
# SHOW DOMAIN USER
rpcclient> enumdomusers

# SHOW DOMAIN USERS AND THEIR INFORMATIONS
rpcclient> querydispinfo

# DOMAIN INFORMATION
querydominfo

# DOMAIN GROUPS
enumdomgroups

# PRINTERS
enumprinters

# REFERENCE
https://www.hackingarticles.in/active-directory-enumeration-rpcclient/
```