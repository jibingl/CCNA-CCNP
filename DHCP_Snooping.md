#

## Two Main functions
1. Inspect DHCP packages for security purpose.
2. Maintaine a table (comtaining IP, MAC, Port, VLAN, and Lease Info) for further usage.


## 
Referening to Peter Paluch's detailed [explanation](https://community.cisco.com/t5/switching/dhcp-snooping/td-p/1622877) posting on Cisco Community, it is been answered clearly why DHCP clients can not get ip addresses as long as enabling **ip dhcp snooping**.  
Below is a summary based on an example network.
### Topology
```
DHCP Server                    ip dhcp snooping                   DHCP Client
   R1                                 S1    
  (+)--g0/0--------------------f0/1--[=]--f0/5-----------------------PC1
                             trusted
```
### Sympotoms
1. As long as *ip dhcp snooping* enalbed on S1, PC1 can not get an IP address by DHCP.
2. You can observe those debugging messages on R1. They indicate the error.
```
R1#debug ip dhcp server packet

DHCP server packet debugging is on.

*May  1 10:11:58.279: DHCPD: inconsistent relay information.
*May  1 10:11:58.279: DHCPD: relay information option exists, but giaddr is zero.
```
3. You can also find debuging message on S1 like below. But they are not related to the problem.
```
S1#debug ip dhcp snooping packet

DHCP Snooping Packet debugging is on

*Mar  1 01:14:20.306: DHCPSNOOP(hlfm_set_if_input): Setting if_input to Fa0/5 for pak.  Was not set
*Mar  1 01:14:20.306: DHCPSNOOP(hlfm_set_if_input): Clearing if_input for pak.  Was Fa0/5
*Mar  1 01:14:20.306: DHCPSNOOP(hlfm_set_if_input): Setting if_input to Fa0/6 for pak.  Was not set
*Mar  1 01:14:20.306: DHCP_SNOOPING: received new DHCP packet from input interface (FastEthernet0/5)
*Mar  1 01:14:20.306: DHCP_SNOOPING: process new DHCP packet, message type: DHCPDISCOVER, input interface: Fa0/5, MAC da: ffff.ffff.ffff, MAC sa: b496.9129.b18d, IP da: 255.255.255.255, IP sa: 0.0.0.0, DHCP ciaddr: 0.0.0.0, DHCP yiaddr: 0.0.0.0, DHCP siaddr: 0.0.0.0, DHCP giaddr: 0.0.0.0, DHCP chaddr: b496.9129.b18d
*Mar  1 01:14:20.306: DHCP_SNOOPING: add relay information option.
*Mar  1 01:14:20.306: DHCP_SNOOPING_SW: Encoding opt82 CID in vlan-mod-port format
*Mar  1 01:14:20.306: DHCP_SNOOPING_SW: Encoding opt82 RID in MAC address format
*Mar  1 01:14:20.315: DHCP_SNOOPING: binary dump of relay info option, length: 20 data:
0x52 0x12 0x1 0x6 0x0 0x4 0x0 0x14 0x1 0x6 0x2 0x8 0x0 0x6 0x0 0x76 0x86 0xAF 0x87 0x0
*Mar  1 01:14:20.315: DHCP_SNOOPING_SW: bridge packet get invalid mat entry: FFFF.FFFF.FFFF, packet is flooded to ingress VLAN: (1)
``` 

### Explanation
1. The PC1 (DHCP client) send out a DHCP request message to switch S1. 
2. The S1 will insert the Option 82 into the client's DHCP request message. However the **giaddr** field of Option 82 remains 0.0.0.0 althogh it is technicaly supposed to be modified, referencing to Option 82 defination, to record the IP address of the relay agent. The reason why the value of **giaddr** remains initial is because S1 is not an DHCP relay agent.  
   > Option 82 is defined as Relay Agent Information in RFC 3046.
3. The switch forwards the request to R1 (DHCP Server).
4. R1 performs a sanity check on the received DHCP message and drops it that contains the Option 82 but whose **giaddr** field is set to 0.0.0.0. Because R1's dropping logic is "how comes that a DHCP message contains the Option 82 (i.e. the DHCP Relay Agent Information Option) when there is no DHCP Relay identified in the GIADDR field?"

### Solution
To configure R1 (DHCP server) to trust the relay info coming form S1. 
```
R1(config)#ip dhcp relay information trust-all            //Global configuration
```
```
R1(config-if)#ip dhcp relay information trusted           //Configuration on interface g0/0
```
