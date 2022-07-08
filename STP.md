# Spanning Tree Protocol

## STP
### _Forming STP_
Interfaces state: blocking -> listening -> learning -> forwarding
```
        root-bridge                       
        SW1_1.1.1.1                           SW2_2.2.2.2          d: designated port (forwarding).
    ------[=]-g0/0-----------------------g1/0-[=]------            r: root port (forwarding).
      f1/0 |   d                           r   | g0/0              nd: non-designated port (blocking).
       d   |                                   |   d
           |                                   |                   
           |                                   |                     
           |                                   |                   
       r   |                                   |   r                
      f1/1 |   d                           nd  | g0/0                
    ------[=]-f1/2-----------------------f1/1-[=]------
         SW3_3.3.3.3                          SW4_4.4.4.4
```
Process to form spanning tree topology:
1. Elect ONE _root bridge_ (lowest bridge-id/BID) which has all interfaces to be _d-port_.
2. Each remaining SW select ONE of its interfaces to be its _r-port_ (lowest root cost -> neighbor BID -> neighbor port id). Ports across from _r-port_ are always _d-port_.
3. Each remaining collision domain select ONE interface to be _d-port_ (lowest root cost -> BID), then other interfaces are _nd-port_.  
### _Configuration_
```
SW1(config)#spanning-tree vlan <vlan-id> root primary              //Set switch's BID as 24576 by default, or less than current-lowest_BID by 4096.
SW1(config)#spanning-tree vlan <vlan-id> root secondary            //Set switch's BID as 28672 by default.

```
## RSTP
### _Forming RSTP_
Interface state: discarding -> learning -> forwarding
```
        root-bridge                       
        SW1_1.1.1.1                           SW2_2.2.2.2          d: designated port (forwarding).
    ------[=]-g0/0-----------------------g1/0-[=]------            r: root port (forwarding).
      f1/0 |   d                           r   | g0/0              al: alternate port (discarding).
       d   |                                   |   d               bk: backup prot (discarding).
           |                                   |                   
           |                                   |                     
           |                                   |                   
       r   |                                   |   r                
      f1/1 |   d                           nd  | g0/0                
    ------[=]-f1/2-----------------------f1/1-[=]------
         SW3_3.3.3.3                          SW4_4.4.4.4
```

## Key Parameters to STP & RSTP
Cost Table
Speed | STP Cost | RSTP Cost |
------|----------|-----------|
10 Mbps |100|2,000,000|
100 Mbps |19|200,000|
1 Gbps |4|20,000|
10 Gbps |2|2,000|
100 Gbps |X|200|
1 Tbps |X|20|

Difference
Paras | Hello Originated | Hello Timer | BPDU Age
---|---|---|---|
STP | Root bridge | 2s | 10*2s
RSTP | All switches | 2s | 3*2s |
