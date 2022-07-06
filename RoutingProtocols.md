# Routing Protocols

## Overview
|Class   |Names|Type|Metric|Admin-Distance|Advertising|IP-Header-Protocol|ADs-Timer|Alg/Called|Balance-path|
|--------|-----|----|------|---|-----------|------------------|-------|----------|------------|
|Static  |     |               |               |1                            |
|Interior|RIPv1|Distance-vector|Hops (Max15)   |120                          |255.255.255.255|         |30s|Routing-by-rumor|1-32(4)|
|Interior|RIPv2|Distance-vector|Hops (Max15)   |120                          |224.0.0.9      |         |30s|Routing-by-rumor|1-32(4)|
|Interior|EIGRP|Hybrid         |Bandwith, delay|5(Summary)/90(Int.)/170(Ext.)|224.0.0.10     |0x58(88) |   |                |1-32(4)|
|Interior|OSPF |Link-state(LSR)|Cost (100M/BW) |110                          |224.0.0.5-6    |0x59(89) |10s|Dijkstra        |1-32(4)|
|Interior|IS-IS|Link-state(LSR)|               |115                          |               |0x7C(124)|   |                |       |
|Exterior|BGP  |Path-vector    |               |20(eBGP)/200(iBGP)           |
|Exterior|EGP  |               |

**Important**: For RIP, EIGRP, and OSPF, `network` cmd doen't tell the router which networks to **advertise**, instead of which interfaces to **activate** routing-protocol on, and then the router will advertise the network prefix of those interfaces.
*****
## RIP (Route Information Protocol) & EIGRP (Enhanced Interior Gateway Routing Protocol)
```
172.16.1.0/28                192.168.2.0/24
  .14|G2/0     10.0.12.0/30        |       
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
### 1. RIP (RIPv1, RIPv2, RIPng)
RIPv1 dosn't support classless/CIDR IP, it means that 10.1.1.0/24 will be become 10.0.0.0
One router = One hop
Routing ads Messages: request and response.
```
R1(config)#router rip 
R1(config-router)#version 2
R1(config-router)#no aoto-summary                   //Default enabled
R1(config-router)#network 10.0.0.0                  //'network' cmd is classful, 10.0.0.0 assumed to 10.0.0.0/8
R1(config-router)#network 172.16.0.0                //Activate RIP on the interfaces fall into the range of network
R1(config-router)#passive-interface g2/0            //Stop sending RIP ads out of the specified interface (G2/0)
R1(config-router)#defualt-information originate     //Ad default gateway out to other RIP adjacent routers
```
### 2. EIGRP
EIGRP is a Hybrid distance-vector routing protocol. Metric calculates bandwith(K1) and delay(K3) by default.
```
R1(config)#router eigrp 1                           //AS (Autonomous System) number must match between routers
R1(config-router)#no aoto-summary
R1(config-router)#passive-interface g2/0            //Stop sending EIGRP ads out of the specified interface (G2/0)
R1(config-router)#network 10.0.0.0                  //'network' cmd can be classful, or
R1(config-router)#network 172.16.1.0 0.0.0.15       //classless
R1(config-router)#eigrp router-id 1.1.1.1           //Router-id priority, manual->loopbak->physical_interface_ip
R1(config-router)#defualt-information originate     //Ad default gateway out to other RIP adjacent routers
```
*******
## OSPF (Open Shortest Path First)
ABR - Area border router. ASBR - Autonomous system boundary router (it connects to external network, normally Internet).
LSA - Link state advertisement. LSDB - Link state database.

Steps to form LSDBs:
1. **Become neighbors** with other routers connected to the same segment;
2. **Exchange** LSAs with neighbor routers;
3. **Calculate** the best routes to each destination, and insert them into the routing table.

_Single-area_ topology:
```
.-------------------------------------AREA 0--.
| 172.16.1.0/28                192.168.2.0/24 |    .---------------------------------.        
|   .14|G2/0     10.0.12.0/30        |G2/0    |    | LSDB                            |
|   R1(+)-G0/0-----------------G0/0-(+)R2     |    | .-----. .-----. .-----. .-----. |       .-----------------------.
|      |G1/0                         |G1/0    |    | | LSA | | LSA | | LSA | | LSA |-|------>| LSA                   |
|      | 10.0.13.0/30                |        |    | `-----` `-----` `-----` `-----` |       | RID:4.4.4.4           |
|      |                             |        |    | .-----. .-----. .-----.         |       | Subnet:192.168.4.0/24 |
|      |                10.0.24.0/30 |        |    | | LSA | | LSA | | LSA |         |       | Cost:1                |
|      |G0/0                         |G0/0    |    | `-----` `-----` `-----`         |       `-----------------------`
|   R3(+)-F2/0-----------------F2/0-(+)R4     |    `---------------------------------` 
|      |G1/0     10.0.34.0/30    G1/0|.254    |    
| 192.168.3.0/25               192.168.4.0/24 |    R1's cost to 192.168.4.0/24 is: 100(R1_g0/0)+100(R2_g1/0)+100(R4_g1/0)=300
`---------------------------------------------`    R1's cost to R2 is: 100(R1_g0/0)+1(R2_lookback0)=101
```
CLI configuration example:
```
R1(config)#router ospf 1                                 //Process ID is locally significant, so routers with different process IDs can become neighbors.
R1(config-router)#network 10.0.12.0 0.0.0.3 area 0
R1(config-router)#network 10.0.13.0 0.0.0.3 area 0
R1(config-router)#network 172.16.1.14 0.0.0.0 area 0
R1(config-router)#passive-interface g2
R1(config-router)#default-information originate
R1(config-router)#router-id 1.1.1.1
R1(config-router)#maximum-path 8                          //Change load-balancing pathes from 4 to 8
R1(config-router)#distance 85                             //Change administrative distance from 110 to 85
R1(config-router)#auto-cost reference-bandwidth 100000    //Default is 100Mbits/s

R1(config)#interface g0/0                                 //Change the cost of interface g0/0
R1(config-if)#ip ospf cost <1-65535>                      //Manualy set the interface OSPF cost, or
R1(config-if)#bandwidth <1-10000000>                      //Change the interface bandwidth (It won't change speed of the interface)
```
Cost calculation formula
```
cost = reference bandwith(bw) / exit-interface bw        #default reference bw is 100Mbits/s
```
Facts of OSPF:
1. An OSPF area is a set of routers and links that share the same LSDB.
2. The _backbone area (area 0)_ is an area that all other areas **must directly connect to**.
3. All areas must have **at least one ABR** connect to the _backbone area_. (Best-practice for design)
4. An ABR connnect maximum of 2 areas. (Best-practice for design)
5. OSPF interfaces in the same subnet must be in the same area. (Best-practice for design)
6. OSPF area should be contigous.
7. _Cost_ to a destination is the total cost of the **outgoing/exit interfaces' costs**.

WRONG OSPF TOPOLOGY DESIGNS:
```
.--------------------------------------area0 (backbone)-.
|-----R0(+)-----------------[=]---------------[=]       |
`----------------------------|-----------------|--------`
                            (+)R2             (+)R3
   .----------area1. .------/|\-area2. .------/|\-area3.
   |    R1(+)------|-|-----/ | \     | |     / | \     |
   |    /  |  \    | |    /  |  \    | |    /  |  \    |
   | (+)  (+)  (+) | | (+)  (+)  (+) | | (+)  (+)  (+) |
   |  |    |    |  | |  |    |    |  | |  |    |    |  |
   `---------------` `---------------` `---------------`
```
| Issues: _area 1_ has no ABR connect to _backbone area (area 0)_; _R2_ connects to too many areas (three areas).
```
.--------------------------------------area0 (backbone)-.
|-----R0(+)--------[=]------------------------[=]       |
`----------------/-----\-----------------------|--------`
            R1(+)       (+)R2                 (+)R3
   .area1----/-----. .-----\----area2. .-------|--area1.
   |      / /\     | |     /\ \      | |     / | \     |
   |    /  |  \    | |    /  |  \    | |    /  |  \    |
   | (+)  (+)  (+) | | (+)  (+)  (+) | | (+)  (+)  (+) |
   |  |    |    |  | |  |    |    |  | |  |    |    |  |
   `---------------` `---------------` `---------------`
```
| Issues: _area 1_ is not contigous, instead it is divided into two parts.
