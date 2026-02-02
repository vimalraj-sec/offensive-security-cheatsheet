## LIST SHARES
```md
# NMAP
sudo nmap --script=nfs-showmount $ip

# showmount
sudo showmount -e $ip
```
## MOUNT SHARE
```md
# STEP 1 CREATE A FOLDER TO MOUNT FIRST
mkdir /mnt/mntshare

# STEP 2 
sudo mount -v -t nfs 192.168.100.20:/srv/nfs /mnt/share

# SYNTAX
sudo mount -v -t nfs $ip:<SHARE> <DIRECTORY>  
sudo mount -v -t nfs -o vers=2 $ip:<SHARE> <DIRECTORY>
sudo mount -t nfs -o vers=3,nolock 10.201.46.20:/users ./razorshare
```
## NFS NMAP SCRIPTS
```md
sudo nmap -p 2049 --script nfs-ls,nfs-showmount,nfs-statfs $ip
```
## METAPLOIT
```md
scanner/nfs/nfsmount #Scan NFS mounts and list permissions
```
## MOUNTING
```md
# LIST SHARES
showmount -e $ip

# MOUNT
mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock

# EXAMPLE
mkdir razorshare
sudo mount -t nfs -o vers=3,nolock 10.201.46.20:/users ./razorshare
```
## NFSShell
```md
https://github.com/NetDirect/nfsshell
https://www.pentestpartners.com/security-blog/using-nfsshell-to-compromise-older-environments/
```
## CONFIG FILES
```md
/etc/exports
/etc/lib/nfs/etab
```
## Privilege Escalation using NFS misconfigurations
```md
https://book.hacktricks.xyz/linux-hardening/privilege-escalation/nfs-no_root_squash-misconfiguration-pe
```
