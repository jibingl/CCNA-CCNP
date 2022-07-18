# CCNA
1. Portsecurity - the behavior of the switch when configure port-security? Under what cuccumstance port will be vilated?
2. IP helper - where should the helper configration reside in?
12. Network design - 3-tier, 2-tier (callopsed core layer)
15. Late collisions?

Router Boost Order:
```
 Power on     Load Bootstrap     POST     Load IOS     Load startup configuration     Generate running configuration
+--------+->-+--------------+->-+----+->-+--------+->-+--------------------------+->-+------------------------------+
                  ROM            ROM       flash              NVRAM                              RAM
```

Table-1 Name Abbrevations
Abbr.| Names | Purposes| Key-concepts|
-----|-------|---------|-------------|
STP  | Spanning Tree Protocol | Loop-free layer2 network | |
DTP  | Dynamic Trunking Protocol | Automatically configure switchport states | |
VTP  | VLAN Trunking Protocol | Centralized VLANs management | |
CDP  | CIsco Discovery Protocol | Exchange device info | |
NTP  | Network Time Protocol | Syncronize time | |
NDP  | Neighbor Discovering Protocol | Gather info required for IPv6 communication| 5 ICMPv6 packet types |

Table-2 MAC Address Table Example
Vlan | Mac Address | Type | Ports |
---|---|---|---|
1 | 00-01-11-22-aa-cc | dynamic | Fa0/1 |

Cisco cmds `show mac address-table`, `clear mac address-table dynamic [address <specific-mac> | interface <inter-id>]`.  
Aging is 5 minutes.

Table-3 ARP Table Examples
Internet Address | Physical Address | Type |
---|---|---|
169.254.255.255 | ff-ff-ff-ff-ff-ff | static |
192.168.1.101   | 00-01-11-22-aa-cc | dynamic |

Table-4 Well-know Multicast Addresses
Purpose | IPv4 Address | IPv6 Address |
---|---|---|
All nodes/hosts (like broadcast) | 224.0.0.1 | FF02::1 |
All routers | 224.0.0.2 | FF02::2 |
All OSPF routers | 224.0.0.5 | FF02::5 |
All OSPF DR/BDRs | 224.0.0.6 | FF02::6 |
All RIP routers | 224.0.0.9 | FF02::9 |
All EICRP routers | 224.0.0.10 | FF02::A |

**IEEE 802** are standards for LAN, PAN, and MAN. The services and protocols specified in IEEE 802 map to the _data link_ and _physical_ layer.
Table-5 IEEE Standards
IEEE | Description |
---|---|
802.3 | Ethernet |
802.11 | WLAN |

Table-6 Network Automation Tools
Name| Client-SRV| OperateModel| CommunicationProtocal| Port| Language| Key Components|
----|-----------|-------------|----------------------|-----|---------|---------------|
Ansible| agentless| push model| SSH| 22| Python| Control Node: Inventory, Template, Variable, Playbook|
Puppet| agent-based; agentless| pull model| HTTP(s)| 8140| Ruby| Puppet Master: Manifest , Templates|
Chef| agent-based| pull model| 10002| Ruby| HTTP(s)| Resources, Recipes, Cookbooks, Run-list|
