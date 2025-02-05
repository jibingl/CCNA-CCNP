# Private VLAN
[**Creating VLANs inside a VLAN**](https://www.cisco.com/c/en/us/support/docs/lan-switching/private-vlans-pvlans-promiscuous-isolated-community/40781-194.html)

Also known as **port isolation**. The private-vlan enabled switch ports within a VLAN can only communicate with a given uplink.  
As a result, direct peer-to-peer traffic between peers through the switch is blocked, and any such communication must go through the uplink. While private VLANs provide isolation between peers at the data link layer, communication at higher layers may still be possible depending on further network configuration.

PVLANs allow the isolation at Layer 2 of devices in the same IP subnet.

### PVLAN Ports
There are three types of PVLAN ports:
 Ports | Name | Explanation |
-------|------|-------------|
**P**  | Promiscuous | Communicates with all other PVLAN ports on the VLAN. | Usually connects to a router | 
**I**  | Isolated | Only communicate with P ports. | Usually connects to hosts. |
**C**  | Community | Communicate with each other and with the promiscuous ports. |

### Rules and Limitations 
`(Updated:September 12, 2024   Cisco Document ID:40781)`

This section provides some rules and limitations for which you must watch when you implement PVLANs.
- PVLANs cannot include VLANs 1 or 1002–1005.
- You must set VLAN Trunk Protocol (VTP) mode to transparent.
- You can only specify one isolated VLAN per primary VLAN.
- You can only designate a VLAN as a PVLAN if that VLAN has no current access port assignments. Remove any ports in that VLAN before you make the VLAN a PVLAN.
- Do not configure PVLAN ports as EtherChannels.
