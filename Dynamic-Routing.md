# Dynamic Routing Protocols
## Overview
```
Class   Names   Algorithm         Metric           Admin-Distance   Advertising-IP       Protocol-NO.   ADs-Timer   Balance-path   Route-table
-----   -----   ---------         ------           --------------   --------------       ------------   ---------   ------------   -----------
IGP     RIP     Distance-vector   Hops(Max15)      120              255.255.255.255(v1)                 30s         1-32(4)        Only neighbors'
                                                                    224.0.0.9(v2)        
IGP     EIGRP   Distance-vector   Bandwith&delay   5(Summary)       224.0.0.10           0x58(88)                   1-32(4)        Only neighbors'
                                                   90(Internal)
                                                   170(External)
IGP     OSPF    Link-state(LSR)   Cost(100M/BW)    110              224.0.0.5(Hello)     0x59(89)       Hello-10s   1-32(4)        Whole network
                                                                    224.0.0.6(DR)                       Dead-40s                   Whole network
IGP     IS-IS   Link-state(LSR)   Cost             115                                   0x7C(124)
EGP     BGP     Path-vector                        20(eBGP)
                                                   200(iBGP)
```
**Important**: For RIP, EIGRP, and OSPF, `network` cmd doen't tell the router which networks to **advertise**, instead of which interfaces to **activate** routing-protocol on, and then the router will advertise the network prefix of those interfaces.
*****
## RIP (Route Information Protocol) & EIGRP (Enhanced Interior Gateway Routing Protocol)
```
172.16.1.0/28                192.168.2.0/24
  .14|g2/0     10.0.12.0/30        |       
  R1(+) ------------------------- (+)R2    
     |                             |       
     | 10.0.13.0/30                |       
     |                             |       
     |                10.0.24.0/30 |       
     |                             |       
  R3(+) ------------------------- (+)R4    
     |         10.0.34.0/30        |       
192.168.3.0/25               192.168.4.0/24
```
#### _RIP (RIPv1, RIPv2, RIPng)_
RIPv1 dosn't support classless/CIDR IP, it means that 10.1.1.0/24 will be become 10.0.0.0  
One router = One hop  
Routing ads Messages: request and response.
```
R1(config)#router rip 
R1(config-router)#version 2
R1(config-router)#no aoto-summary                   //Default enabled
R1(config-router)#network 10.0.0.0                  //'network' cmd is classful, 10.0.0.0 assumed to 10.0.0.0/8
R1(config-router)#network 172.16.0.0                //Activate RIP on the interfaces fall into the range of network
R1(config-router)#passive-interface g2/0            //Stop sending RIP ads out of the specified interface (g2/0)
R1(config-router)#defualt-information originate     //Ad default gateway out to other RIP adjacent routers
```
#### _EIGRP_
EIGRP is a Hybrid distance-vector routing protocol.  
Metric calculates bandwith(K1) and delay(K3) by default.
```
R1(config)#router eigrp 1                           //AS (Autonomous System) number must match between routers
R1(config-router)#no aoto-summary
R1(config-router)#passive-interface g2/0            //Stop sending EIGRP ads out of the specified interface (G2/0)
R1(config-router)#network 10.0.0.0                  //'network' cmd can be classful, or
R1(config-router)#network 172.16.1.0 0.0.0.15       //classless
R1(config-router)#eigrp router-id 1.1.1.1           //Router-id priority: manual --> loopbak --> physical_interface_ip
R1(config-router)#defualt-information originate     //Ad default gateway out to other RIP adjacent routers
```
*******
## OSPF (Open Shortest Path First)
Refer to (_OSPF.md_)(https://github.com/jibingl/CCNA/edit/main/OSPF.md) for detail knowledge.
