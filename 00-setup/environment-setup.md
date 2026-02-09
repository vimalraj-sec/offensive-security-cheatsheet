#### Create Working Directories
```bash
mkdir nmap fuzz www exploits dump scan; touch 01-machineip.txt 02-credentials.txt
```
#### Set Target Variables - Save the target IP to the file 
```bash
nano 01-machineip.txt
```
#### Export commonly used environment variables
```bash
export ip=$(cat 01-machineip.txt);export url=http://$ip
```
#### Verify variables
```bash
echo $ip;echo -e;echo $url
```
