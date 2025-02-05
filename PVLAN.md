# Private VLAN
**Creating VLANs inside a VLAN**

Also known as **port isolation**. The private-vlan enabled switch ports within a VLAN can only communicate with a given uplink.  

As a result, direct peer-to-peer traffic between peers through the switch is blocked, and any such communication must go through the uplink. While private VLANs provide isolation between peers at the data link layer, communication at higher layers may still be possible depending on further network configuration.


### PVLAN Ports
There are three types of PVLAN ports:
 Ports | Name | Explanation |
-------|------|-------------|
**P**  | Promiscuous | Allowed to send and receive frames from any other port on the VLAN. | Usually connects to a router | 
**I**  | Isolated | Allowed only to communicate with P ports – they are “stub”. | Usually connects to hosts. |
**C**  | Community | Allowed to talk to their buddies within the same community group, addition to P ports. |

