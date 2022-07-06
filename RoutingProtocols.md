# Routing Protocols

### Overview

|Classification|Names|Type|Metric|Distance|Advertising|IP-Header-Protocol|info-ad|Algorithms|Balance-path|
|----|---|---|---|---|---|---|---|---|---|
|Interior|RIPv1|Distance-vector|Hops (Max15)|120|255.255.255.255| |every 30s|Routing-by-rumor|1-32(4)|
|Interior|RIPv2|Distance-vector|Hops (Max15)|120|224.0.0.9| |every 30s|Routing-by-rumor|1-32(4)|
|Interior|EIGRP|Hybrid|Bandwith, delay|90, 170|224.0.0.10| | |0x58(88)| |1-32(4)|
|Interior|OSPF|Link-state (LSR)|Cost (100M/BW)|110|224.0.0.5,224.0.0.6|0x59(89)| |Dijkstra|1-32(4)|
|Interior|IS-IS|Link-state (LSR)| | | | |0x7C(124)|
|Exterior|BGP|Path-vector|
|Exterior|EGP|

**Important**: For RIP, EIGRP, and OSPF, `network` cmd doen't tell the router which networks to **advertise**, instead of which interfaces to **activate** routing-protocol on, and then the router will advertise the network prefix of those interfaces.

### RIP (Route Information Protocol) & EIGRP (Enhanced Interior Gateway Routing Protocol)
```
172.16.1.0/28                192.168.2.0/24
     |                             |       
  .14|G2/0     10.0.12.0/30        |       
    (+) R1---------------------R2 (+)      
     |                             |       
     | 10.0.13.0/30                |       
     |                             |       
     |                10.0.24.0/30 |       
     |                             |       
    (+) R3---------------------R4 (+)      
     |         10.0.34.0/30        |       
     |                             |       
192.168.3.0/25               192.168.4.0/24
```
#### 1. RIP (RIPv1, RIPv2, RIPng)
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
#### 2. EIGRP
EIGRP is a Hybrid distance-vector routing protocol. Metric calculates bandwith(K1) and delay(K3) by default.
```
R1(config)#router eigrp 1                           //AS (Autonomous System) number must match between routers
R1(config-router)#no aoto-summary
R1(config-router)#passive-interface g2/0            //Stop sending EIGRP ads out of the specified interface (G2/0)
R1(config-router)#network 10.0.0.0                  //'network' cmd can be classful,
R1(config-router)#network 172.16.1.0 0.0.0.15       //or classless

R1(config-router)#defualt-information originate     //Ad default gateway out to other RIP adjacent routers
```

### OSPF (Open Shortest Path First)

ABR - Area boarder router. ASBR - Autonomous system boundary router (it connects to external network, normally Internet).
LSA - Link state advertisement. LSDB - Link state database.
```
.---------------------------------.        
| LSDB                            |
| .-----. .-----. .-----. .-----. |            .-------------------------------.
| | LSA | | LSA | | LSA | | LSA |-|----------> | LSA                           |
| `-----` `-----` `-----` `-----` |            | RID:<router-id>               |
| .-----. .-----. .-----.         |            | Subnet:<ad-interface-ip/mask> |
| | LSA | | LSA | | LSA |         |            | Cost:<to-destination-cost>    |
| `-----` `-----` `-----`         |            `-------------------------------`
`---------------------------------` 
```
Steps to form LSDBs:
1. **Become neighbors** with other routers connected to the same segment;
2. **Exchange** LSAs with neighbor routers;
3. **Calculate** the best routes to each destination, and insert them into the routing table.

Cost calculation formula
```
cost = reference bandwith(bw) / exit-interface bw        #default reference bw is 100Mbits/s
```

Facts for OSPF
1. An OSPF area is a set of routers and links that share the same LSDB.
2. All areas must have **at least one ABR** connect to the backbone area. (Best-practice for design)
3. OSPF interfaces in the same subnet must be in the same area. (Best-practice for design)
4. OSPF area should be contigous.
5. The OSPF _process ID_ is **locally significant**, so routers with different _process IDs_ can become neighbors.
6. _Cost_ to a destination is the total cost of the **outgoing/exit interfaces' costs**.

CLI configuration example:
```

```
