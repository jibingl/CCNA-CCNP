# Spanning Tree Protocol
Form a loop-free switch network (layer 2) by exchanging BPDU messages and block data traffic on specific interfaces.  
 __BID Format__
```
                    +--------------Bridge ID--------------------------+
                    |  BP (16 bits)  |        MAC (32 bits)           |
                    +----------------+--------------------------------+
                                |
+-------------------------------+-----------------------------------------------------------------------------------------------+
|     Bridge Priority (4 bit)   |                       Extended System ID  (12 bits)                                           |
+-------------------------------+-----------------------------------------------------------------------------------------------+
| 32768 | 16384 | 8192  | 4096  | 2048  | 1024  |  512  |  256  |  128  |  64   |  32   |  16   |   8   |   4   |   2   |   1   |
|   1   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   0   |   1   |
+-------------------------------------------------------------------------------------------------------------------------------+
```
## STP (802.1D)
### _Forming STP_
1. Elect __ONE__ _root bridge_ which has all interfaces to be _d-port_.
    > Default _bridge priority_ is 32768 on all switches, so the MAC address is used as the tie-breaker (lowest MAC becomes the _root bridge_).
2. Each remaining switch selects __ONE__ of its interfaces to be its _r-port_ which always connect to _d-port_. 
3. Each remaining collision domain selects __ONE__ interface to be _d-port_, then other interfaces are _nd-port_. 

Election (*Lowest*) | 1st | 2nd | 3rd | 4th |
----------|-----|-----|-----|-----|
1 - Root Bridge /LAN | BID |
2 - Root Port /Switch| Root Cost | Neighbor BID | Neighbor Port ID | Local Port ID |
3 - Designated Port /Segment | Root Cost | Local BID | Local Port ID |

```
        root-bridge                       
        SW1-BID_32769                        SW2-BID_32769
        MAC_a.a.a                             MAC_b.b.b
    ------[=]-g0/0-----------------------g1/0-[=]------            d: designated port
      f1/0 |   d                           r   | g0/0              r: root port
       d   |                                   |   d               nd: non-designated port
           |                                   |                   
       r   |                                   |   r               Note: MAC addresses are simplfied. 
      f1/1 |   nd                          d   | g0/0                
    ------[=]-f1/2-----------------------f1/1-[=]------
         SW3-BID_32769                       SW4-BID_32769
         MAC_c.c.c                           MAC_d.d.d
```
### _Cost Table_
Speed | STP Cost | RSTP Cost |
------|----------|-----------|
10 Mbps |100|2,000,000|
100 Mbps |19|200,000|
1 Gbps |4|20,000|
10 Gbps |2|2,000|
100 Gbps |X|200|
1 Tbps |X|20|

### _Interfaces Roles and States_
Interfaces Roles | Designated      | Root            | Non-Designated |
-----------------|-----------------|-----------------|----------------|
BPDUs            | send/forward    | reveive         | receive |
Data             | receive/forward | receive/forward | drop |

Interfaces States | blocking | listening       | learning       | forwarding |
------------------|----------|-----------------|----------------|------------|
BPDUs             | recieve  | recieve/forward | send/receive   | send/recieve | 
Data              | drop     | drop            | only learn MAC | send/receive/learn MAC |
Timer             | N/A      | 15s             | 15s            | N/A |

### _portfast & bpduguard_
The command `SW(config-if)#spanning-tree portfast` quickly moves access ports to _forwarding_ by passing _listening_ and _learning_. However, it may lead to loop links when a new switch connects to this fast-port. To solve the issue, `SW(config-if)#spanning-tree bpduguard` is used to block BPDU packets coming into the fast-port.
## RSTP (802.1W)
### _Forming RSTP_
Steps and process are the same as STP.
```
        root-bridge (primary)                     (root secondary)
        SW1-BID_24577                             SW2-BID_28673         
    ------[=]-g0/0----------------------------g1/0-[=]------            d: designated port
      f1/0 |   d                                r   | g0/0              r: root port
       d   |                                        |   d               al: alternate port
           |                                        |                   b: backup prot
       r   |                                        |   r                           
      f1/1 |   d                               al   | g0/0              UplinkFast: al-port move to forwarding immediately.
    ------[=]-f1/2------------[Hub]-----------f1/1-[=]------            
          b| f2/2               |                 SW4-BID_32769
           `--------------------`                 MAC_d.d.d
         SW3-BID_32769
         MAC_c.c.c
```
### _Interfaces States_
Interfaces States | discarding | learning | forwarding |
---|---|---|---|
BPDUs | recieve | recieve/forward | send/receive | 
Data | drop | only learn MAC | send/receive/learn MAC |
Timer | N/A | 15s | N/A |

## Comparation between STP and RSTP
Paras | Hello Originated | Hello Timer | BPDU Age | BPDU des_MAC |
---|---|---|---|---|
STP/PVST | Root bridge | 2s | 10\*2s| 0180-c200-0000 |
RSTP/PVST+ | All switches | 2s | 3\*2s | 0100-0ccc-cccd |

## Spanning Tree Load-Balancing
PVST/PVST+ stands for Per-Vlan Spanning Tree which can be used to balance layer 2 traffic by implying different STP setting on each VLAN.
```
                            PC_vlan10                 root-B     PC_vlan10
                  SW1       |                          SW1       |                      SW1       
ISP1------(+)-----[=]------[=]----PC_vlan20            [=]-d--r-[=]                     [=]-d--al[=]----PC_vlan20
           |       | \    /                            d| \d   /al                      r| \d   /r 
           |       |   \/                               |   \/                           |   \/
           |       |   /\                               |   /\                           |   /\
           |       | /    \                            r| /d   \r                       d| /d   \al
ISP2------(+)-----[=]------[=]----PC_vlan10            [=]-d--al[=]----PC_vlan10        [=]-d---r[=]
                  SW2       |                          SW2                              SW2       |
                            PC_vlan20                                                  root-B     PC_vlan20

SW1(config)#spanning-tree vlan 10 root primary              //Set switch's BID as 24576 by default, or less than current-lowest_BID by 4096.
SW1(config)#spanning-tree vlan 20 root secondary            //Set switch's BID as 28672 by default.

SW2(config)#spanning-tree vlan 20 root primary
SW2(config)#spanning-tree vlan 10 root secondary
```
