## CHECKS
```md      
# CHECK FOR PASSWORDS IN FILES ON MOUNTABLE DRIVES
```
## INFO COMMANDS
```md
# CHECK GENERAL RPC INFO 
sudo rpcinfo $ip  

# LIST ALL MOUNTS
showmount -e $ip
     
# CREATE A DIRECTORY
sudo mkdir /var/backups

# MOUNT NFS
sudo mount -t nfs 192.168.100.6:/backups /var/backups  
OR
sudo mount -t nfs $ip:/backups /var/backups -nolock

# REMOUNT
mount -t nfs -o remount /mnt/nfs  

umount /mnt/nfs  
umount -f /mnt/nfs  
umount -l /mnt/nfs  
umount -lf /mnt/nfs  
```