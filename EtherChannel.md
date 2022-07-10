# EtherChannel
Aka _Port Channel_ or _Link Aggregation Croup (LAG)_  
Class |Full-name |Property |Up to Interfaces|
---|---|---|---|
PAgP| Port Aggregation Protocol| Cisco| 8|
LACP| Link Aggregation Control Protocol| Public| 8/16|
Static| Static EtherChannel| | 8|

## Syntax
```
SW1(config)#interface [range] {interface | interface-range}
SW1(config-if-range)#channel-group <number> mode <eth-channel-mode>     //channel-group number locally significant in the switch.
                                    1-255         auto|desirable        //For PAgP.
                                                  active|passive        //For LACP.
                                                  on                    //Static.
```

## EtherChannel Forming Table
Modes         |     auto/passive |  desirable/active |  on
---|---|---|---|
**auto/passive**    |   X           |   Eth-channel    |    X
**desirable/active**  | Eth-channel  |  Eth-channel     |   X    
**on**               |  X            |  X                |  Eth-channel 
## Configuration Requiements
All member-interfaces must have matching configurations:
  - Same duplex (full/half);
  - Same speed;
  - Same switchport mode (access/trunk)
  - Same allowed VLANs/native VLAN (for trunk interfaces)  

If not matching, the mis-config interface will be excluded while EtherChannel still works with other member-interfaces matching the requirements.

## Load Banlance Methods
Methods used to calculate in EtherChannel load balancing can be configured as one of `dst-ip, dst-mac, src-dst-ip, src-dst-mac, src-ip, src-mac`.
Example:
```
SW1#show etherchannel load-balance
EtherChannel Load-Balancing Configuration:
      src-dst-ip
EtherChannel Load-Balancing Addresses Used Per-Protocol:
Non-IP: Source XOR Destination MAC address
  IPv4: Source XOR Destination IP address
  IPv6: Source XOR Destination IP address

SW1#conf t
SW1(config)#port-channel load-balance sc-dst-mac
SW1(config)#do show etherchannel load-blalance
EtherChannel Load-Balancing Configuration:
      src-dst-mac
EtherChannel Load-Balancing Addresses Used Per-Protocol:
Non-IP: Source XOR Destination MAC address
  IPv4: Source XOR Destination MAC address
  IPv6: Source XOR Destination MAC address
```
