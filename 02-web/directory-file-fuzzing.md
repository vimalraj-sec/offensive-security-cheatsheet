## 🔹 common.txt (Quick Initial Scan)

```bash
sudo ffuf -r -c -w /usr/share/wordlists/dirb/common.txt -fc 404 -u $url/FUZZ | tee fuzz/ffuf-common
sudo wfuzz -c -w /usr/share/wordlists/dirb/common.txt -v -t 40 --hc 404 -u $url/FUZZ | tee fuzz/wfuzz-common.txt
sudo gobuster dir -w /usr/share/wordlists/dirb/common.txt -b 404 -o fuzz/gobuster-common.txt -t 20 -u $url/
sudo feroxbuster -w /usr/share/wordlists/dirb/common.txt -C 404 -o fuzz/feroxbuster-common.txt -t 20 -u $url/
```
## 🔹 raft-large-files.txt (File Discovery)

```bash
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt -fc 404 -u $url/FUZZ | tee fuzz/ffuf-raft-large-files
sudo wfuzz -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt -v -t 40 --hc 404 -u $url/FUZZ | tee fuzz/wfuzz-raft-large-files.txt
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt -b 404 -o fuzz/gobuster-raft-large-files.txt -t 20 -u $url/
sudo feroxbuster -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt -C 404 -o fuzz/feroxbuster-raft-large-files.txt -t 20 -u $url/
```
## 🔹 raft-large-directories.txt (Directory Discovery)

```bash
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -fc 404 -u $url/FUZZ/ | tee fuzz/ffuf-raft-large-directories
sudo wfuzz -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -v -t 40 --hc 404 -u $url/FUZZ/ | tee fuzz/wfuzz-raft-large-directories.txt
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -b 404 -o fuzz/gobuster-raft-large-directories.txt -t 20 -u $url/
sudo feroxbuster -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -C 404 -o fuzz/feroxbuster-raft-large-directories.txt -t 20 -u $url/
```

## 🔹 directory-list-2.3-medium.txt (Medium Depth Scan)

```bash
sudo ffuf -r -c -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -fc 404 -u $url/FUZZ/ | tee fuzz/ffuf-directory-list-2.3-medium.txt
sudo wfuzz -c -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -v -t 40 --hc 404 -u $url/FUZZ/ | tee fuzz/wfuzz-directory-list-2.3-medium.txt
sudo gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -b 404 -o fuzz/gobuster-directory-list-2.3-medium.txt -t 20 -u $url/
sudo feroxbuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -C 404 -o fuzz/feroxbuster-directory-list-2.3-medium.txt -t 20 -u $url/
```

### With Extensions

```bash
sudo gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -b 404 -o fuzz/gobuster-directory-list-2.3-medium.txt -x php,html,txt -t 20 -u $url/
```

## 🔹 raft-large-words.txt (Word-Based Discovery)

```bash
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -fc 404 -u $url/FUZZ | tee fuzz/ffuf-raft-large-words
sudo wfuzz -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -v -t 40 --hc 404 -u $url/FUZZ | tee fuzz/wfuzz-raft-large-words.txt
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -b 404 -o fuzz/gobuster-raft-large-words.txt -u $url/
sudo feroxbuster -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -b 404 -o fuzz/feroxbuster-raft-large-words.txt -u $url/
```

## 🔹 big.txt (Aggressive Scan)

```bash
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -fc 404 -u $url/FUZZ | tee fuzz/ffuf-big
sudo wfuzz -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -v -t 40 --hc 404 -u $url/FUZZ | tee fuzz/wfuzz-big
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/big.txt -b 404 -o fuzz/gobuster-big -u $url/
sudo feroxbuster -w /usr/share/seclists/Discovery/Web-Content/big.txt -b 404 -o fuzz/feroxbuster-big -u $url/
```

## 🔹 Subdomain / VHost Fuzzing

```bash
ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 40 -fc 404 -H "Host: FUZZ.domain.com" -u http://domain.com/ | tee fuzz/ffuf-subdomain
sudo wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -v -t 40 --hc 404 -H "Host: FUZZ.domain.com" -u http://domain.com | tee fuzz/wfuzz-subdomain
```

## 🔹 Extension Bruteforcing

```bash
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -fc 404 -e .php,.txt -u $url/FUZZ | tee fuzz/ffuf-big-ext
sudo ffuf -r -c -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -fc 404 -e .php,.html,.txt -u $url/FUZZ/ | tee fuzz/ffuf-medium-ext
sudo ffuf -r -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -fc 404 -u $url/FUZZ -e .php,.txt,.bak,.js,.asp,.aspx -o fuzz/raft-large-words-extensions -of md
sudo gobuster dir -w /usr/share/wordlists/dirb/common.txt -b 404 -o fuzz/gobuster-common-ext.txt -t 20 -u $url/ -x php,html,htm
sudo feroxbuster -w /usr/share/wordlists/dirb/common.txt -C 404 -o fuzz/feroxbuster-common-ext.txt -t 20 -u $url/ -x php,html,htm
```

## 🔹 Response Filtering Reference

### ffuf Filters

```
-fc  Filter by status code
-fl  Filter by line count
-fw  Filter by word count
-fs  Filter by size
-fr  Filter by regex
-fmode  Set filter mode
```

### wfuzz Filters

```
--hc  Hide status code
--hl  Hide lines
--hw  Hide words
--hh  Hide characters
```
