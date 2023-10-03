# Virtual LAN
\- Limiting broadcast domains in layer2/switched LAN.  
\- The distinctions between _VLAN_ and _SVI_ is layer2 vs layer3.    
\- VLAN configuration is locally significant. It means VLAN config on one switch does not have to match another switch connected to it. 
\- Native VLAN is configured per trunk and is locally significant, which means different trunk link can have different native vlan.

## 802.1Q
**802.1Q** is one of two trunking ptotocol, the other one is ISL (Cisco property).

The 802.1Q tage is insert into a 802.3/Ethernet frame between _source MAC address_ and _type/length_.
```
+------------+------------++++++++++++-----------+---------------------+----------+
|  Dest-MAC  |  Src-MAC   |  802.1q  |Type/Length|         DATA        | FCS(CRC) |
|   6 bytes  |   6 bytes  |  4 bytes |  2 bytes  |                     | 4 bytes  |  
+------------+------------++++++++++++-----------+---------------------+----------+ 
                                |
                                v
 0                 1                   2                   3
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            TPID               |     |D|                       |
|   (Tag Protocol Identifier)   | PCP |E|       VID             |
|           0x8100              |     |I|    (Vlan ID 0-4096)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
 > `PCP`, `DEI`, and `VID` are together called **TCI** (Tag Control Information).  

Names   | Bits    | Functions |
--------|---------|-----------|
**TPID**| 16 bits | Always 0x8100 indicates the frame is 802.1Q-taged |
**PCP** | 3 bits  | Priority Code Point - QoS |
**DEI** | 1 bit   | Droping or not when network congested |
**VID** | 12 bits | Vlan ID of the destination. Value 0-4096/1-4094 |

## Tagged/Trunk & Untagged/Access Ports
```text
           +=========================================================================SWITCH=====================+
           |                                                                                                    |
           |                                  +-----vlan_buckets-------+                                        |
   >>>>> ACCESS_PORT >>>>>                    |      (untagged)        |                          >>>>> ACCESS_PORT >>>> 
--->---untagged?----YES-->------------------> | :````````````````````: | ------------------------->-------------------->--- untagged_frame
   >            `---NO--->---drop             | :   vlan_1_bucket    : | ------.         .-------->-------------------->--- untagged_frame
   >>>>>>>>>>>>>>>>>>>>>>>                    | :   vlan_2_bucket    : |        \       /         >>>>>>>>>>>>>>>>>>>>>> 
           |                                  | :        ...         : |          \   /                         |
           |                                  | :   vlan_n_bucket    : |            \/                          |
           |                                  | :        ...         : |           /  \                         |
   >>>>> TRUNK_PORT >>>>>              .----> | :        ...         : |         /      \         >>>>> TRUNK_PORT >>>>> 
--->--- tagged? ---YES-->--strip_tag---`      | :````````````````````: |--------`        `-------->----insert_tag------>--- tagged_frame
   >            `--NO--->-------------------> | : native_vlan_bucket : |-------------------------->-------------------->--- untagged_frame
   >>>>>>>>>>>>>>>>>>>>>>                     | :....................: |                          >>>>>>>>>>>>>>>>>>>>>> 
           |                                  +------------------------+                                         |
           |                                                                                                     |
           +=====================================================================================================+
```

## ROAS (Router on a Stack)
Leverage on `subinterfaces` created on a physical interface. 
```
R1#config t
R1(config)#interface g0/1
R1(config-if)#no shutdown

R1(config-subif)#interface g0/1.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config-subif)#exit

R1(config)#interface g0/1.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0
```
There are two ways to confiure **native vlan** on a router of ROAS.  
#1 - Configuring on the subinferface:
```
R1(config)#interface g0/1.5
R1(config-subif)#encapsulation dot1q 5 native
R1(config-subif)#ip address 192.168.5.1 255.255.255.0
```
#2 - Confiuring ip address on the physical interface directly:
```
R1(config)#interface g0/1
R1(config-if)#ip address 192.168.5.1 255.255.255.0
```

## Multilayer Switch
Levarage on `SVI` (Switch Virtual Interface)
```
SW1(config)#ip routing
SW1(config)#vlan 10
SW1(config-vlan)#vlan 20

SW1(config-vlan)#interface vlan 10
SW1(config-if)#ip address 192.168.10.1 255.255.255.0
SW1(config-if)#no shutdown
SW1(config-if)#interface vlan 20
SW1(config-if)#ip address 192.168.20.1 255.255.255.0
SW1(config-if)#no shutdown

SW1(config-if)#interface g0/0
SW1(config-if)#no switchport
SW1(config-if)#ip address 211.10.10.2 255.255.255.252
SW1(config-if)#exit
SW1(config)#ip route 0.0.0.0 0.0.0.0 211.10.10.1
```
