## CREATE FILES AND FOLDERS
```bash
mkdir nmap fuzz www exploits dump scan; touch 01-machineip.txt 02-credentials.txt
```
---
## SETTING VARIABLES
```bash
nano 01-machineip.txt
```
```bash
export ip=$(cat 01-machineip.txt);export url=http://$ip
```
```bash
export url=http://$ip
```
```bash
export ip=$(cat 01-machineip.txt)
```
```bash
echo $ip;echo -e;echo $url
```

