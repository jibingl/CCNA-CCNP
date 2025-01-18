# CCNA & CCNP

📢 This is the study notes of my personal CCNA learning. Most of contents retrived from the Youtube channel **[Jeremy's IT Lab](https://www.youtube.com/c/JeremysITLab)**. 

## Common Knowledge
Router Boost Order:
```
 Power on     Load Bootstrap     POST     Load IOS     Load startup configuration     Generate running configuration
+--------+->-+--------------+->-+----+->-+--------+->-+--------------------------+->-+------------------------------+
                  ROM            ROM       flash              NVRAM                              RAM
```

📎 Table-1 Name Abbrevations
Abbr.| Names                         | Purposes                                   | Key-concepts                                |
-----|-------------------------------|--------------------------------------------|---------------------------------------------|
STP  | Spanning Tree Protocol        | Loop-free layer2 network                   | PortFast, BPDU guard/filter, Root/Loop guard|
DTP  | Dynamic Trunking Protocol     | Automatically configure switchport states  | Trunk/Access ports, native vlan |
VTP  | VLAN Trunking Protocol        | Centralized VLANs management               | v2 vs v3            |
CDP  | CIsco Discovery Protocol      | Exchange device info                       | Layer 2               |
NTP  | Network Time Protocol         | Syncronize time                            | Defualt stratum 8     |
NDP  | Neighbor Discovering Protocol | Gather info required for IPv6 communication| 5 ICMPv6 packet types |
UDLD | Unidirectional Link Detection | Help STP to prevent layer2 loops           | Unidirectional link |

📎 Table-2 Well-know Multicast Addresses
Purpose                          |IPv4 Address|IPv6 Address|
---------------------------------|------------|------------|
All nodes/hosts (like broadcast) | 224.0.0.1  | FF02::1    |
All routers                      | 224.0.0.2  | FF02::2    |
All OSPF routers                 | 224.0.0.5  | FF02::5    |
All OSPF DR/BDRs                 | 224.0.0.6  | FF02::6    |
All RIP routers                  | 224.0.0.9  | FF02::9    |
All EIGRP routers                | 224.0.0.10 | FF02::A    |

📎 Table-3 Network Automation Tools
Name   | Client-SRV            | Op-Model  |Protocol| Port | By-Lang| Use-Lang       | Key Components                                       |
-------|-----------------------|-----------|--------|------|--------|----------------|------------------------------------------------------|
Ansible| agentless             | push model| SSH    | 22   | Python | YAML           | Control Node: Inventory, Template, Variable, Playbook|
Puppet | agent-based; agentless| pull model| HTTP(s)| 8140 | Ruby   |Declarative lang|Puppet Master: Manifest , Templates                   |
Chef   | agent-based           | pull model| HTTP(s)| 10002| Ruby   |                | Resources, Recipes, Cookbooks, Run-list              |
