# Secure Configurations to Cisco Devices

## Basic security config
  ```
  config t
  service password-encryption
  service time-stamps
  enable secret ccna
  
  line console 0
  password cisco
  login
  exit
  ```
## Secure Login SSH
  ```
  config t
  enable secret ccna
  hostname SW1
  ip domain-name mydomain.ca
  ip ssh version 2
  access-list 99 permit host 192.168.1.99
  username ciscoadmin secret ccna
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
  ```
  config t
  interface f0/0
  switchport mode access                  //Explicitly set access ports
  switchport nonegotiate                  //Disable DTP negotiate on access ports
  ```
Attack #2 - double tagging (vlan hopping) 
  ```
  config t
  switchport mode trunk
  switchport trunk native vlan 99         //Change native vlan (default vlan 1) to another unused vlan ID
  ```
  
