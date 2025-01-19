# Spanning Tree Protocol
Form a loop-free switch network (layer 2) by exchanging BPDU messages and block data traffic on specific interfaces.  
 __BID Format__
```
                    +-------------Bridge ID--------------------------+
                    |  BP (16-bit)  |         MAC (32-bit)           |
                    +---------------+--------------------------------+
                            ||
       0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
     +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
     |   BP      |        Extended System ID         |
     +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+

     BP - Bridge Priority (65536, 32768, 16384, ..., 4, 2, 1)
```
## 🌲 STP (802.1D)
### Forming STP (3-step)
1. Elect __ONE__ ***root bridge*** which has all interfaces to be _d-port_.
2. Each remaining switch selects __ONE__ of its interfaces to be its ***r-port*** which always connect to _d-port_. 
3. Each remaining collision domain selects __ONE__ interface to be ***d-port***, then other interfaces are _nd-port_.  

Election (*Lowest*)                 | --> | --> | --> | --> |
------------------------------------|-----|-----|-----|-----|
1 - **Root Bridge** per LAN         | BID |
2 - **Root Port** per switch        | Root Cost | Neighbor BID | Neighbor Port ID | Local Port ID |
3 - **Designated Port** per segment | Root Cost | Local BID | Local Port ID |
> Default _bridge priority_ is 32768 on all switches, so the MAC address is used as the tie-breaker to determine the _root bridge_.

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

### Interfaces Roles and States
Interfaces Roles | Designated      | Root            | Non-Designated |
-----------------|-----------------|-----------------|----------------|
BPDUs            | send/forward    | reveive         | receive        |
Data             | receive/forward | receive/forward | drop           |

Interfaces States | blocking | listening       | learning       | forwarding |
------------------|----------|-----------------|----------------|------------|
BPDUs             | recieve  | recieve/forward | send/receive   | send/recieve | 
Data              | drop     | drop            | only learn MAC | send/receive/learn MAC |
Timer             | N/A      | 15s             | 15s            | N/A |

## 🌲 PortFast & BPDU Guard
 Names         | Purposes                                                                             | STP function  | When recieve a BPDU,                                           |
---------------|--------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------|
**PortFast**   | Move access ports to _forwarding_ by passing _listening_ and _learning_              | Sending BPDUs | disable PortFast and transfer the port to normal STP operation.|
**BPDU Guard** | Protect against unauthorized switches being connected to ports intended to end hosts | Sending BPDUs | disable the port (errdisable).                                 |
**BPDU Filter**| Block ports from sending BPDUs                                                       | Disabled      | ignore recieved BPDU packets. (Only effect on per-port config) |
### _Configuration combinations table_
Configration to a port    | Pros | Cons |
--------------------------|------|------|
`portfast`                | - Immediately forward data <br> - Auto-transfer to normal STP port if a new switch connected | - Potential STP attack |
`bpduguard`               | - No impact to existing STP topology if unexpected switches connected | - Cause the port disabled & need to be enabled manually <br> - Wait for 30s to forward data |
`portfast` + `bpduguard`  | - Immediately forward data <br> - No impact to existing STP topology if unexpected switches connected | - Cause the port disabled & need to be enabled manually |
`portfast` + `bpdufilter` | - Immediately forward data <br> - No impact to existing STP topology if unexpected switches connected | - Cannot transfer to normal STP port automatically |


## 🌲 RSTP (802.1W)
### Forming RSTP (3-step)
Steps and processes are the same as STP.
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
### Interfaces States
Interfaces States | discarding | learning | forwarding |
---|---|---|---|
BPDUs | recieve | recieve/forward | send/receive | 
Data | drop | only learn MAC | send/receive/learn MAC |
Timer | N/A | 15s | N/A |

### RSTP _vs_ STP
Paras | Hello Originated | Hello Timer | BPDU Age | BPDU des_MAC |
------|------------------|-------------|----------|--------------|
STP/PVST | Root bridge | 2s | 10\*2s| 0180-c200-0000 |
RSTP/PVST+ | All switches | 2s | 3\*2s | 0100-0ccc-cccd |

### Cost Table
Speed | STP Cost | RSTP Cost |
------|----------|-----------|
10 Mbps |100|2,000,000|
100 Mbps |19|200,000|
1 Gbps |4|20,000|
10 Gbps |2|2,000|
100 Gbps |X|200|
1 Tbps |X|20|

## 🌲 Spanning Tree Load-Balancing
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
## 🌲 UplinkFast & BackboneFast

 Features    | Functioning                                                                 | Skipped Timers       | Save Time | Implement/Configure practice |
-------------|-----------------------------------------------------------------------------|----------------------|-----------|------------------------------|
UplinkFast   | Convert nd-port from block to forward immediately                           | Listening & Learning | 30S       | Only switches with nd ports  |
BackboneFast | Send/Receive RLQ Request/Response to Root Bridge for checking inferior BPDU | Max Age              | ~20S      | All switches                 |

![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/f39b4313-633f-4700-bee3-067f03514821)

## QoA
**1. What dose the command `spanning-tree vlan 1 root primary` do?**  
     - __primary__ = set priority to 24576, or 4096 lower than the current Root Bridge (if setting the priority to 24576 wouldn't make this switch the Root).  
**2. Can *primary* gurantee the Root?**  
     - No. To gurantte the Root, set priority to zero by command `spanning-tree vlan 1 priority 0`.  
