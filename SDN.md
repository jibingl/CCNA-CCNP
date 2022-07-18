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
**SBI** traffics can be OpenFlow, onePK, OpFlex, and NETCONF.

 Notes | Data Plane | Controll PLane | Management Plane |
-------|------------|----------------|------------------|
aka | forwarding plane | | |
functions | Data forwarding | How to forward data | Manage Devices |
if effect forwarding? | Yes | Yes | No |
examples | routing, forwarding, NAT, ACLs, port security| routing table, ARP table, MAC address table, STP | SSH, Telnet, Syslog, SNMP, NTP |
