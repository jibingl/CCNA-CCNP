# CCNA

1. ~~Routing protocols: OSPF, RIPv2, EIGRP~~
2. ~~Addrivatation-terms table: STP, VTP, DTP, CDP...~~
3. ~~Router ID/Interface election summary table~~
4. Protocol headers - drawing fun - IP header, Ehternet packet, ...
5. ~~NAT - inside local, inside global, outside local, outside global (draw a pic)~~
6. ~~Stats/Processing for some protocols, like STP, OSPF~~
7. Router boost order, loading IOS sequence, and each of storage's usage?
8. WLAN
9. SDN
10. REST and ?
11. QoS
12. Network design - 3-tier, 2-tier (callopsed core layer)
13. UDP vs TCP, suitable to what situations?
14. EtherChannel


Abbreviations| Names | Purposes| Key-concepts|
---|---|---|---|
STP | Spanning Tree Protocol | Loop-free layer2 network | |
DTP | Dynamic Trunking Protocol | Automatically configure switchport states | |
VTP | VLAN Trunking Protocol | Centralized VLANs management | |
CDP | CIsco Discovery Protocol | Exchange device info | |
NTP | Network Time Protocol | Syncronize time | |

MAC Address Table
Vlan | Mac Address | Type | Ports |
---|---|---|---|
1 | 00-01-11-22-aa-cc | dynamic | Fa0/1 |

Cisco cmds `show mac address-table`, `clear mac address-table dynamic [address <specific-mac> | interface <inter-id>]`.  
Aging is 5 minutes.

ARP Table
Internet Address | Physical Address | Type |
---|---|---|
169.254.255.255 | ff-ff-ff-ff-ff-ff | static |
192.168.1.101   | 00-01-11-22-aa-cc | dynamic |
