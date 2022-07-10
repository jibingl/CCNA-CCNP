# First Hop Redundancy Protocols
Enable two or more routers to protect/backup _default-gateway_ for a subnetwork.

Abbreviation | Full-name | Property | Router Mode| MulticastIP | vMAC | Load Balancing |
---|---|---|---|---|--|---|
HSRP | Hot Standby Reouter Protocol | Cisco | active/standby | 224.0.0.2/102 | 0000.0c07.acXX  0000.0c9f.fXXX | Among subnets
VRRP | Virtual Router Redundancy protocol |Open-standard | master/backup | 224.0.0.18 | 0000.5e00.01XX | Among subnets
GLBP | Gateway Load Balancing Protocol | Cisco | AVG/AVF | 224.0.0.102 | 0007.b400.XXYY | Within ONE subnet

## HSRP
Typicial Topology
```
           R2             SW2          SW4
ISP1------(+)-g0/0_.252--------[=]----------[=]-------.4--PC4
         standby               | \        /  `-------.3--PC3
                               |   \    /
               vIP_.254        |     \/     172.16.0.0/24    default-getaway_.254
                               |     /\
                               |   /    \
         active                | /        \  .--------.2--PC2
ISP2------(+)-g0/0_.253--------[=]----------[=]--------.1--PC1
           R1             SW1          SW3 
```
After configuration, HSRP will generates a _virtual MAC_, and then broadcast this vMAC out to keep SWs' arp table up tp date when failure occured. The routers membered of HSRP use reserved multicast IP address to keeep communicating by sendding send Hello msg.  
### Configuration Example
```
R1(config)#interface g0/0
R1(config-if)#standby version 2                    //Optional
R1(config-if)#standby 1 ip 172.16.0.254            //'1' is HSRP group number. THe range of HSRP's version1 is 0-255, v2 is 0-4095. 
R1(config-if)#standby 1 priority 200               //Default is 100 <0-255>. (Highest priority will be active router; if tied, highest IP)
R1(config-if)#standby 1 preempt                    //Take over the role of active anyway

R2(config-if)#standby version 2                    //Routers MUST match the same version
R2(config-if)#standby 1 ip 172.16.0.254
```
### HSRP Load Balancing
Load balancing among different subnets/vlans. Configure each router to be different active/standby mode for each subnet/vlan.

## GLBP Load Balancing
Load balancing among different partial addresses within **ONE** subnet/vlan.
