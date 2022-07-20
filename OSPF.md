# OSPF (Open Shortest Path First)
Steps to form LSDBs (Link State Database):
 1. **Become neighbors** with other routers connected to the same segment;
 2. **Exchange** LSAs (Link State Advertisement) with neighbor routers;
 3. **Calculate** the best routes to each destination, and insert them into the routing table.  
### _Key-info & Configuration_  
Topology example and Key-info:
```
                                                  WAN_144.14.14.1/30
               .---area 0------------------------------|-----------.    
               |    R2-id_2.2.2.2                    R1|id_1.1.1.1 |
192.168.2.0/24 |  DR             10.0.10.0/30          |g0/0       | 172.16.1.1   .---------------------area 1-----------------------------. 
         [=]---|-g1/1-(+)-g0/0-DR------------BDR-g1/1-(+)-f1/0-DR-.2----[=]       |     R8-id_8.8.8.8                       R7-id_7.7.7.7  |
               |    BDR|f1/0                        BDR|g2/2       |              |     (+)-g0/0--------\     /--------g0/0-(+)-g1/1---[=] |
               |       |                               |           |              |          DR          \   /          BDR      DR        |
               |       | 10.0.20.0/30                  |           |              |                       \ /                              |
               |       |                  10.0.40.0/30 |           |              |                       [=] 172.16.0.0/24                |
               |       |                               |           |              |                       / \                              |
               |     DR|f1/0                         DR|g2/2       | 10.0.50.0/30 |         DROther      /   \        DROther    DR        |
         [=]---|-g1/1-(+)-g0/0-BDR------------DR-g0/0-(+)-s1/0-----|-ospf-ppp-net-|-s1/0-(+)-g0/0-------/     \--------g0/0-(+)-g1/1---[=] |
192.168.3.0/24 |  DR              10.0.30.0/30       DR|g1/1       |              |       R5-id_5.5.5.5                     R6-id_6.6.6.6  |
               |    R3-id_3.3.3.3                    R4|id_4.4.4.4 |              `-------------------ospf-broadcast-net-------------------` 
               `---------------ospf-broadcast-net------|-----------`
                                                      [=]192.168.4.0/24
                                                                                
  R2#show ip ospf database                                                       *Routes cost calculation (reference-bw 100000):
 .-------------------------------------.                                        ..........................................................
 | LSDB (area0)                        |       .---routers generate-.           | Route 1    = R1_g1/1 -> R2_g1/1_192.168.2.0            |
 |           Router LSAs               |       | RID:3.3.3.3        |           |  Cost 200  =   100   +    100                          |
 | .------. .------. .------. .------. |       | Net:192.168.3.0/24 |           | Route 2    = R1_g1/1 -> R2_lookback                    |
 | | LSA1 | | LSA1 | | LSA1 | | LSA1 |-|------>| Cost:100           |           |  Cost 101  =   100   +     1                           |
 | `------` `------` `------` `------` |       `--------------------`           | Route 3    = R1_g1/1 -> R2_f1/0 -> R3_g1/1_192.168.3.0 |
 | .------. .------. .------. .------. |       .---routers generate-.           |  Cost 1200 =   100   +    1000  +    100               |
 | | LSA1 | | LSA1 | | LSA1 | | LSA1 |-|------>| RID:3.3.3.3        |           ``````````````````````````````````````````````````````````
 | `------` `------` `------` `------` |       | Net:10.0.20.0/30   |             *Cost forluma = reference-bw / exit-interface-bw
 |           Network LSAs              |       | Cost:1000          |
 | .------. .------. .------.          |       `--------------------`
 | | LSA2 | | LSA2 | | LSA2 |----------|------>.---DR generates-----.
 | `------` `------` `------`          |       | DR-RID:3.3.3.3     |
 |         AS-external LSA             |       `--------------------`
 | .------.                            |       .---ASBR generates---.
 | | LSA5 |----------------------------|------>| ASBR RID:1.1.1.1   |
 | `------`                            |       `--------------------`
 `-------------------------------------`       

*ASBR (Autonomous system boundary router) is R1 that connects to external network, normally Internet.
*ABR (Area Border Router) is R5 or R4, depends on context, that connects two areas.
*In OSPF broastcast network, DR/BDR election rule (High-Low): interface-priority --> router-id --> interface-ip.
*When DR is down, current BDR becomes new DR even though existing of another higher-priority interface that will become new BDR.
```
CLI configuration example:
```
R1(config)#router ospf 1                                  //Process ID is locally significant.Rrouters with different process IDs can become neighbors.
R1(config-router)#network 10.0.10.0 0.0.0.3 area 0
R1(config-router)#network 10.0.40.0 0.0.0.3 area 0
R1(config-router)#network 172.16.1.2 0.0.0.0 area 0
R1(config-router)#passive-interface f1/0
R1(config-router)#default-information originate
R1(config-router)#router-id 1.1.1.1
R1(config-router)#maximum-path 8                          //Change load-balancing pathes from 4 to 8
R1(config-router)#distance 85                             //Change administrative distance from 110 to 85
R1(config-router)#auto-cost reference-bandwidth 100000    //Default is 100Mbits/s
R1(config-router)#passive-interface default               //Set all interfaces to passive, then
R1(config-router)#no passive-interface g1/1               //specify an interface as active
R1(config-router)#shutdown                                //Shut the OSPF process 1 down

R1(config)#interface g1/1
R1(config-if)#ip ospf 1 area 0                            //Active OSPF directly on an interface
R1(config-if)#ip ospf cost <1-65535>                      //Manualy set the interface OSPF cost, or
R1(config-if)#bandwidth <1-10000000>                      //Change the interface bandwidth (It won't change speed of the interface)
R1(config-if)#ip ospf priority <0-255>                    //Change interface priority (defualt is 1)
R1(config-if)#ip ospf hello-interval <1-65535>            //Change Hello message sending interval (default 10s)
R1(config-if)#ip ospf dead-interval <1-65535>             //Change Dead timer-count (default 40s)
R1(config-if)#ip ospf network broadcast                   //Set as broadcast OSPF network. Ohter types are point-to-point, non-broadcast, and point-to-multipoint
R1(config-if)#ip ospf authentication-key <string>         //Set password for OSPF neighbors communication
R1(config-if)#ip ospf authentication                      //Enable
```
### _Facts of OSPF_
1) An OSPF area is a set of routers and links that share the same LSDB.
2) The _backbone area (area 0)_ is an area that all other areas **must directly connect to**.
3) All areas must have **at least one ABR** connect to the _backbone area_. (Best-practice for design)
4) An ABR connnect maximum of 2 areas. (Best-practice for design)
5) OSPF interfaces in the same subnet must be in the same area. (Best-practice for design)
6) OSPF area should be contigous.
7) _Cost_ to a destination is the total cost of the **outgoing/exit interfaces' costs**.  
### _Troubleshooting_
NEIGHBOR REQUIREMENTS:  
1) Area number must match
2) Interfaces must be the same subnet
3) No shutdown to OSPF process
4) Hello/Dead intervals must match
5) Router ID must unique
6) Authentication settings must match
7) OSPF Network type must match
8) IP MTU settings must match
  > Under _7_ and _8_, neighbors can be formed but OSPF operate inproperly.  

WRONG OSPF TOPOLOGY DESIGNS:
```
.--------------------------------------area0 (backbone)-.
|-----R0(+)-----------------[=]---------------[=]       |
`----------------------------|-----------------|--------`
        R1(+)---------------(+)R2             (+)R3
   .------/|\-area1. .------/|\-area2. .------/|\-area3.
   |     / | \     | |     / | \     | |     / | \     |
   |    /  |  \    | |    /  |  \    | |    /  |  \    |
   | (+)  (+)  (+) | | (+)  (+)  (+) | | (+)  (+)  (+) |
   |  |    |    |  | |  |    |    |  | |  |    |    |  |
   `---------------` `---------------` `---------------`
```
  > Issues: _area 1_ has no ABR connect to _backbone area (area 0)_; _R2_ connects to too many areas (three areas).
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
  > Issues: _area 1_ is not contigous, instead it is divided into two parts.
