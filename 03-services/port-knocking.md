## CHECKS

```md
# Service appears filtered / closed
# No obvious open ports but host is alive
# Firewall likely controlling access
# Suspect port knocking sequence required
```
## INSTALL KNOCK CLIENT

```bash
sudo apt install knockd -y
```
## BASIC PORT KNOCK

```bash
# Example Knock Sequence
knock $ip 4000 5000 6000

# Try Different Port Orders
knock $ip 6000 5000 4000
knock $ip 5000 4000 6000
```
## VERIFY WITH NMAP

```bash
# Initial Scan (Before Knock)
sudo nmap -p- $ip

# Perform Knock
knock $ip 4000 5000 6000

# Scan Again (After Knock)
sudo nmap -p- $ip
```

If new port opens (e.g., 22) → port knocking successful.
## AUTOMATED KNOCK BRUTE (Manual Method)

```bash
# Example: Try Common Sequences
for seq in "1111 2222 3333" "4000 5000 6000" "7000 8000 9000"; do
  knock $ip $seq
  sleep 1
  sudo nmap -p 22 $ip
done
```
## REFERENCE

```md
https://github.com/pha5matis/Pentesting-Guide/blob/master/port_knocking.md
```
