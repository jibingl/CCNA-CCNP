# Secure Configurations to Cisco Devices
- Warning banners
- Set encrypted passwords
- Enable passwords check when login
- Only allow SSH access
- Add time stamps
- Shutdown unused ports and assign them to a black-hole vlan
- Port-security

## Basic security config
Banner, passwords encryption and requirements, login pwds check, time stamps. 
```
config t
banner c Authorized Access Only! c
service password-encryption
service time-stamps                           //Add time stamps to logs.
security password min-length 8                //Value range 1-16. Some devices don't support.
enable secret ciscoenpass

line console 0
password ciscoconpass
login
exit
```
## Secure Login SSH
```
config t
enable secret ciscoenpass
hostname SW1
ip domain-name mydomain.ca
ip ssh version 2
access-list 99 permit host 192.168.1.99
username ciscoadmin secret ciscoadminpass
crypto key generate rsa modulus 4096

line vty 0 15
login local
exec-timeout 5 0
transport input ssh
access-class 99 in
end
```
## Prevent VLAN-hopping Attacks
Attack #1 - switch spoofing  
Countermeasure: Disable DTP on access ports by explicitly setting switchport mode.
```
config t
interface f0/0
switchport mode access                  //Explicitly set access ports
switchport nonegotiate                  //Disable DTP negotiate on access ports
```
Attack #2 - double tagging (vlan hopping)  
Countermeasure: Change native vlan to another vlan other than vlan 1.
```
config t
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 99         //Change native vlan (default vlan 1) to another unused vlan ID
```
  
