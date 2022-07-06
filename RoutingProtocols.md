# Routing Protocols

### Overview

|Classification|Names|Metric|Routing Cal|Admin-cost|Advertising|Algorithms|IP-Header-Protocol|
|----|---|---|---|---|---|---|---|
|Interior|RIPv2|Distance-vector|Hops (Max15)|120|224.0.0.9|
|Interior|EIGRP|Hybrid|Bandwith, delay|90, 170|224.0.0.10| |0x58(88)|
|Interior|OSPF|Link-state (LSR)|Cost (100M/BW)|110|224.0.0.5,224.0.0.6|Dijkstra|0x59(89)|
|Interior|IS-IS|Link-state (LSR)| | | | |0x7C(124)|
|Exterior|BGP|Path-vector|
|Exterior|EGP|

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

### RIPv2


### EIGRP



