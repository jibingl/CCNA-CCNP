# CCNA

8. WLAN
9. SDN
10. REST and ?
12. Network design - 3-tier, 2-tier (callopsed core layer)
15. Late collisions?

Router Boost Order:
```
 Power on     Load Bootstrap     POST     Load IOS     Load startup configuration     Generate running configuration
+--------+->-+--------------+->-+----+->-+--------+->-+--------------------------+->-+------------------------------+
                  ROM            ROM       flash              NVRAM                              RAM
```

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

Well-know Multicast Address
Purpose | IPv4 Address | IPv6 Address |
---|---|---|
All nodes/hosts (like broadcast) | 224.0.0.1 | FF02::1 |
All routers | 224.0.0.2 | FF02::2 |
All OSPF routers | 224.0.0.5 | FF02::5 |
All OSPF DR/BDRs | 224.0.0.6 | FF02::6 |
All RIP routers | 224.0.0.9 | FF02::9 |
All EICRP routers | 224.0.0.10 | FF02::A |
