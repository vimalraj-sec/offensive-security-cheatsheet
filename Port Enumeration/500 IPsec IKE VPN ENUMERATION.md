## REFERENCE
```md
https://book.hacktricks.xyz/network-services-pentesting/ipsec-ike-vpn-pentesting
```
## NMAP
```md
sudo nmap -sU -p 500 $ip -oN nmap/scan-port500
```
## IKESCAN
```md
sudo ike-scan -M $ip

# Check the Line SA 
- Example
SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration(4)=0x00007080)
Enc=3DES
Hash=SHA1
Group=2:modp1024 
Auth=PSK 
LifeType=Seconds 
LifeDuration(4)=0x00007080

# If there is a field called **AUTH** with the value **PSK**. This means that the vpn is configured using a preshared key

- 0 returned handshake; 0 returned notify: This means the target is not an IPsec gateway.
- 1 returned handshake; 0 returned notify: This means the target is configured for IPsec and is willing to perform IKE negotiation, and either one or more of the transforms you proposed are acceptable (a valid transform will be shown in the output).
- 0 returned handshake; 1 returned notify: VPN gateways respond with a notify message when none of the transforms are acceptable (though some gateways do not, in which case further analysis and a revised proposal should be tried).
```
## IPSEC TUNNELING APPLICATION
```md
# strongswan
sudo apt install strongswan -y

# Check the config file
- man ipsec.secrets - For more information

# config file
sudo nano /etc/ipsec.secrets

- Need PSK - password

# check ipsec.conf file
man ipsec.conf

# strongswan vpn tunnel config
nano /etc/ipsec.conf
--------------------------------------------------
conn Conceal                         
                type=transport
                keyexchange=ikev1            
                left=10.10.14.47              
                leftprotoport=tcp
                right=10.129.239.79
                rightprotoport=tcp            
                authby=psk
                fragmentation=yes            
                esp=3des-sha1                
                ike=3des-sha1-modp1024       
                ikelifetime=8h               
                auto=start
---------------------------------------------------

ifconfig tun0 mtu 1000

# Start tunnel 
ipsec start --nofork

# Chek the Ports by nmap TCP scan
sudo nmap -sT -p- $ip

```
