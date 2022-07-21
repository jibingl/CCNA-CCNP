# Software-Defined Networking
An networking approach that centralizes the _control plane_ into an application called a _controller_.

SDN Architecture & Network Automation
```
   SDN Architecture                                             Network Automation
                                   +------------+
                                   |    Apps    |
                                   +------------+
   Application Layer                   |     |                   Management Plane
   ---------------------------------( REST API )---------------------------------
                                       | NBI |
                                +------+-----+-----+
                                |     Controller   |
                                | .--------------. |
                                | |Controll Plane| |
                                | `--------------` |
                                +------------------+
   Controll Layer                  |     SBI    |                   Controll Plane
   --------------------------------|--( API )---|---------------------------------
   Infrastructure Layer           /              \                      Data Plane
                         +---(+)---+            +---(+)---+
                         |   R1    |            |   R2    |
                         | .-----. |            | .-----. |
                         | |Data | |            | |Data | |
           ---(packet)---| |Plane| |--(packet)--| |Plane| |---(packet)---
                         | `-----` |            | `-----` |
                         +---------+            +---------+
```
 > Notes: NBI (Northbound Interface) - controller-to-devices; SBI (Southbound Interface) - human/apps-to-contoller.  
**SBI** traffics can be OpenFlow, onePK, OpFlex, NETCONF, RESTCONF.

 Notes | Data Plane | Controll PLane | Management Plane |
-------|------------|----------------|------------------|
AKA | forwarding plane | | |
Functions | Data forwarding | How to forward data | Manage Devices |
If effect forwarding? | Yes | Yes | No |
Examples | routing (match to routing-table), forwarding (match to MAC-table), de- & re-encapsulation, add & remove 802.1q tags, VPN (add new IP), NAT (changing IPs), drop due to ACLs or port-security| matain routing table, ARP table, MAC address table, STP | SSH, Telnet, Syslog, SNMP, NTP |
