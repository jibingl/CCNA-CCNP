# CCNA

8. WLAN
9. SDN
10. REST and ?
11. QoS
12. Network design - 3-tier, 2-tier (callopsed core layer)
13. UDP vs TCP, suitable to what situations?

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
