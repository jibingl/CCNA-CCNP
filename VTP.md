# VLAN Trunking Protocol

Mode           | Modify Vlan | Forward VTP msg | Syncronize Vlan
---------------|-------------|-----------------|----------------
**server**     | Yes         | Yes             | Yes
**client**     | No          | Yes             | Yes
**transparent**| Yes         | Yes             | No

 > Notes: To forward VTP msg (vlan datebase), transparent-switch must join the same VTP domain.

## Reset _Reversion_ Number of a switch to 0
SW1(config)#vtp mode transparent                 //Method 1: change to transparent mode
SW1(config)#vtp domain <unsued-domain-name>      //Method 2: change to an unused domain

## Troubleshoot VTP Propegetion Failures
1. VTP information only passes through a trunk port.
2. Make sure that if EtherChannels are created between two switches, only Layer 2 EtherChannels propagate VLAN information.
3. Make sure that the VLANs are active in all the devices.
4. One of the switches must be the VTP server in a VTP domain. 
5. The VTP domain name must match and it is case sensitive. 
6. Make sure that no password is set between the server and client, or the same on both sides.
7. Every switch in the VTP domain must use the same VTP version.
8. Switches that operate in transparent mode drop VTP advertisements if they are not in the same VTP domain.
9. The extended-range VLANs are not propagated. 
10. The Security Association Identifier (SAID) values must be unique.
11. The updates from a VTP server do not get updated on a client if the client already has a higher VTP revision number. 
